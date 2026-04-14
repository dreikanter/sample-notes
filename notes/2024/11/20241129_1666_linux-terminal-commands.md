---
title: Linux terminal commands I forget
slug: linux-terminal-commands
tags: [linux, terminal, reference, tools]
---

# Linux terminal commands I forget

Personal reference for commands that I use infrequently enough to always need to look up.

## Process management

```bash
ps aux                          # All processes
ps aux | grep nginx             # Find process by name
lsof -i :3000                   # What's using port 3000
kill -9 <pid>                   # Force kill
pkill -f "node server.js"       # Kill by name pattern
htop                            # Interactive process viewer
```

## Disk and files

```bash
df -h                           # Disk usage by filesystem
du -sh /path/to/dir             # Size of directory
du -sh *                        # Size of each item in current dir
ncdu /                          # Interactive disk usage browser
ls -lah                         # List with sizes
stat filename                   # File metadata (permissions, timestamps)
```

## Network

```bash
curl -I https://example.com     # Headers only
curl -v ...                     # Verbose (shows request + response)
wget -O output.html https://... # Download to file
nc -zv hostname 5432            # Test if port is reachable
ss -tlnp                        # Listening TCP sockets (modern netstat)
dig example.com                 # DNS lookup
```

## Text processing

```bash
sort -k2 -n file.txt            # Sort by second column numerically
uniq -c                         # Count unique lines (requires sorted input)
awk '{print $1}' file.txt       # Print first column
wc -l file.txt                  # Line count
tr ',' '\n' file.csv            # Replace commas with newlines
```

## System info

```bash
uname -a                        # Kernel/OS info
free -h                         # Memory usage
uptime                          # Load average and uptime
who                             # Logged in users
last                            # Login history
```

Good reference for the advanced stuff: [The Linux Command Line by William Shotts](https://linuxcommand.org/tlcl.php) — freely available online.
