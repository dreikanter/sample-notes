---
title: SSH config patterns
slug: ssh-config-patterns
tags: [ssh, linux, reference]
---

# SSH config patterns

Useful entries for `~/.ssh/config` that I keep forgetting and rediscovering.

## Basic host alias

```
Host myserver
  HostName 192.168.1.100
  User ubuntu
  IdentityFile ~/.ssh/my_key
  Port 22
```

Then: `ssh myserver`

## Jump host (bastion pattern)

```
Host internal-server
  HostName 10.0.0.50
  User ec2-user
  ProxyJump bastion.example.com
```

The ProxyJump directive replaces the old ProxyCommand pattern in modern OpenSSH.

## Keep connections alive

```
Host *
  ServerAliveInterval 60
  ServerAliveCountMax 3
```

Prevents connections from dropping when idle. ServerAliveInterval sends a keepalive every 60 seconds; CountMax sets how many missed responses before giving up.

## Multiplexing (reuse connections)

```
Host *
  ControlMaster auto
  ControlPath ~/.ssh/control/%r@%h:%p
  ControlPersist 10m
```

The first SSH connection to a host creates a master socket; subsequent connections reuse it. Speeds up repeated connections dramatically. Create the control directory first:

```bash
mkdir -p ~/.ssh/control
chmod 700 ~/.ssh/control
```

## Useful flags to know

- `-v` / `-vv` / `-vvv`: verbose output for debugging
- `-N`: don't execute remote command (for tunnel-only sessions)
- `-L localport:remotehost:remoteport`: local port forwarding
- `-R remoteport:localhost:localport`: remote port forwarding

OpenSSH documentation: [https://man.openbsd.org/ssh_config](https://man.openbsd.org/ssh_config)
