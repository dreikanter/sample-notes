---
title: Linux process tools — quick reference
slug: linux-process-tools
tags: [linux, sysadmin, reference]
---

# Linux process tools — quick reference

**`ps` — snapshot of running processes**

```bash
ps aux           # All processes, BSD format
ps -ef           # All processes, full format
ps aux --sort=-%mem | head -10  # Top 10 by memory
```

**`top` / `htop`**

`htop` is preferred for interactive use — colour output, tree view, easy kill. `top` is available on more systems without installation.

Useful `top` keys: `M` sort by memory, `P` sort by CPU, `k` kill process.

**`lsof` — open files and network connections**

```bash
lsof -i :8080     # What's using port 8080?
lsof -p 1234      # Files opened by process 1234
lsof -u username  # Files opened by user
```

**`strace` — system call tracing**

```bash
strace -p 1234    # Attach to running process
strace -e trace=open,read python script.py  # Filter by syscall type
```

**`kill` and signals**

```bash
kill -15 1234   # SIGTERM — polite stop
kill -9 1234    # SIGKILL — force stop (no cleanup)
kill -1 1234    # SIGHUP — reload config (for daemons)
killall nginx   # Kill by name
```

**`/proc` filesystem**

```bash
cat /proc/1234/cmdline  # Full command line of process
cat /proc/1234/status   # Memory, threads, etc.
ls /proc/1234/fd/       # Open file descriptors
```

**`systemctl` for services**

```bash
systemctl status nginx
systemctl restart nginx
journalctl -u nginx -f   # Follow logs
```

[Linux man pages online](https://man7.org/linux/man-pages/)
