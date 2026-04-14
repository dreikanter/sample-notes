---
title: SSH keys and config reference
slug: ssh-and-keys-reference
tags: [ssh, security, reference]
---

# SSH keys and config reference

Rebuilt my SSH setup this month as part of the year-start cleanup (see [[20231231_1349]]). Notes on what I changed and why.

## Key generation

```bash
ssh-keygen -t ed25519 -C "comment describing the key"
```

Ed25519 is the current recommendation — faster than RSA, smaller key, equally secure for practical purposes. RSA 4096 is still fine if you have compatibility requirements with old systems.

## SSH config file (`~/.ssh/config`)

```
Host work-server
    HostName server.example.com
    User myusername
    IdentityFile ~/.ssh/id_ed25519_work
    AddKeysToAgent yes

Host github.com
    IdentityFile ~/.ssh/id_ed25519_github
    AddKeysToAgent yes

Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
```

The `Host *` block applies to all connections. `ServerAliveInterval` prevents "broken pipe" errors on idle connections.

## Key management

```bash
ssh-add ~/.ssh/id_ed25519_work    # add to SSH agent
ssh-add -l                         # list loaded keys
ssh-add -D                         # remove all keys from agent
```

On macOS, add `UseKeychain yes` to the config block and keys persist across reboots in the system keychain.

## Security notes

- Never share private keys (obvious, but)
- Use a passphrase on private keys — macOS Keychain and ssh-agent mean you only type it once per session
- `ssh-copy-id` is the right way to deploy public keys to servers, not manual editing of authorized_keys
- Rotate keys when access changes (person leaves, system is compromised)

Reference: [Mozilla's SSH guidelines](https://infosec.mozilla.org/guidelines/openssh.html) for hardened server configuration recommendations.
