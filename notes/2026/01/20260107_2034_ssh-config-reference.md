# SSH config reference

A running reference for my `~/.ssh/config` setup and common patterns.

## Basic host alias

```
Host myserver
    HostName 203.0.113.42
    User deploy
    IdentityFile ~/.ssh/id_ed25519_deploy
    Port 2222
```

Then `ssh myserver` instead of typing the full command each time.

## Jump hosts (bastion)

```
Host internal-server
    HostName 10.0.1.5
    User app
    ProxyJump bastion.example.com
```

Or with an explicit bastion definition:

```
Host bastion
    HostName bastion.example.com
    User admin
    IdentityFile ~/.ssh/id_ed25519_bastion

Host internal-*
    ProxyJump bastion
    User app
```

The wildcard `internal-*` matches any host starting with `internal-`.

## Useful global options

```
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    AddKeysToAgent yes
    IdentityFile ~/.ssh/id_ed25519
```

`ServerAliveInterval` prevents dropped connections through NAT. `AddKeysToAgent` means you only enter the passphrase once per session.

## Key generation (modern)

```bash
ssh-keygen -t ed25519 -C "comment describing key purpose"
```

Ed25519 over RSA for new keys — smaller, faster, same or better security.

## Copying public key

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@host
```

Reference: https://man.openbsd.org/ssh_config
Good guide: https://www.digitalocean.com/community/tutorials/how-to-configure-custom-connection-options-for-your-ssh-client
