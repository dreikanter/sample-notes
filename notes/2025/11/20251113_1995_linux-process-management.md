---
title: Linux process management reference
slug: linux-process-management
tags: [linux, sysadmin, reference]
---

# Linux process management reference

Commands I use for diagnosing running processes.

## Viewing processes

```bash
ps aux            # all processes, user-oriented format
ps aux | grep app # filter by name
pgrep nginx       # returns PIDs matching name
pidof nginx       # PIDs for exact binary name
```

## Process tree

```bash
pstree -p         # show tree with PIDs
pstree -u         # show tree with users
```

## Resource usage

```bash
top               # interactive; press 'q' to quit
htop              # better interactive viewer (if installed)
ps aux --sort=-%mem | head -20  # top 20 by memory
ps aux --sort=-%cpu | head -20  # top 20 by CPU
```

## Signals

```bash
kill -SIGTERM 1234    # graceful stop (15)
kill -SIGKILL 1234    # force kill (9)
kill -SIGHUP 1234     # reload config (many daemons)
killall nginx         # kill all processes named nginx
pkill -f "python app.py"  # kill by full command match
```

Always try SIGTERM before SIGKILL. SIGKILL doesn't allow cleanup.

## Background and foreground

```bash
command &          # run in background
Ctrl+Z             # suspend foreground process
bg                 # resume suspended in background
fg                 # bring background to foreground
jobs               # list background jobs
```

## lsof — list open files

```bash
lsof -p 1234              # files open by PID 1234
lsof -i :8080             # process listening on port 8080
lsof -u username          # files open by user
```

## /proc filesystem

Direct process information without tools:

```bash
cat /proc/1234/cmdline    # command that started the process
cat /proc/1234/status     # memory, state, parent PID
ls -la /proc/1234/fd/     # open file descriptors
```

Reference: [man ps](https://man7.org/linux/man-pages/man1/ps.1.html) and the [Linux kernel /proc docs](https://www.kernel.org/doc/Documentation/filesystems/proc.txt).
