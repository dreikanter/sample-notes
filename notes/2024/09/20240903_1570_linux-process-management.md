---
title: Linux process management cheatsheet
slug: linux-process-management
tags: [linux, sysadmin, reference, cli]
description: Process management commands and signals reference
---

# Linux process management cheatsheet

Commands I use often enough to need a reference but not often enough to always remember.

## Finding processes

```bash
ps aux | grep <name>       # find by name
pgrep <name>               # just the PIDs
pgrep -la <name>           # PIDs and command lines
lsof -i :8080              # what's listening on port 8080
ss -tlnp                   # all listening sockets with PIDs (modern alternative to netstat)
```

## Signals

```bash
kill -15 <pid>    # SIGTERM - polite shutdown request
kill -9 <pid>     # SIGKILL - immediate kill (process has no say)
kill -1 <pid>     # SIGHUP - reload config (many daemons handle this)
kill -0 <pid>     # check if process exists without signalling it

pkill <name>      # send signal by name (SIGTERM by default)
killall <name>    # similar, slightly different matching
```

Prefer SIGTERM. Only use SIGKILL when SIGTERM has failed and you've waited. Some processes do important cleanup on SIGTERM.

## Priority and nice values

```bash
nice -n 10 <command>          # start with lower priority (10 is moderately low)
renice -n 5 -p <pid>          # change priority of running process
```

Nice values: -20 (highest priority) to 19 (lowest). Only root can set negative values.

## Background and foreground

```bash
<command> &        # start in background
Ctrl-Z             # suspend current foreground job
bg                 # continue suspended job in background
fg                 # bring background job to foreground
jobs               # list current shell's background jobs
nohup <cmd> &      # background, immune to hangup signal
```

## Systemd units (for services)

```bash
systemctl status <service>
systemctl restart <service>
systemctl journalctl -u <service> -f   # follow logs
```

Reference: [Linux man pages](https://man7.org/linux/man-pages/)
