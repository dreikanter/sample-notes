# SSH config tricks

The `~/.ssh/config` file is underused. These patterns save me constant typing.

## Basic host alias

```
Host myserver
    HostName 192.168.1.100
    User deploy
    Port 2222
    IdentityFile ~/.ssh/deploy_key
```

Now `ssh myserver` instead of `ssh -p 2222 -i ~/.ssh/deploy_key deploy@192.168.1.100`.

## Jump hosts (bastion)

```
Host internal-server
    HostName 10.0.0.50
    User app
    ProxyJump bastion.example.com
```

Or multi-hop:
```
Host deep-internal
    ProxyJump bastion.example.com,internal-server
```

## Wildcard patterns

```
Host *.internal.example.com
    User deploy
    IdentityFile ~/.ssh/internal_key
    StrictHostKeyChecking no

Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    AddKeysToAgent yes
    IdentityFile ~/.ssh/id_ed25519
```

`Host *` settings apply as defaults — useful for things like keepalive.

## Reuse connections

```
Host *
    ControlMaster auto
    ControlPath ~/.ssh/sockets/%r@%h-%p
    ControlPersist 600
```

Creates a multiplexed connection — subsequent SSH/SCP to the same host reuse the existing connection. Much faster for multiple operations.

Create the sockets directory: `mkdir -p ~/.ssh/sockets`

## Git over SSH with non-standard port

```
Host github-alt
    HostName ssh.github.com
    Port 443
    User git
    IdentityFile ~/.ssh/github_key
```

Then use `github-alt:user/repo.git` as the remote URL.

Docs: https://man7.org/linux/man-pages/man5/ssh_config.5.html
