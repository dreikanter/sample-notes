# Linux process management reference

Commands I use for debugging running processes and managing system resources.

## Process inspection

```bash
ps aux                          # all processes, BSD format
ps -ef                          # all processes, POSIX format
ps aux | grep myapp             # find specific process
pgrep myapp                     # just the PIDs
pgrep -la myapp                 # PIDs with names

top                             # interactive process viewer
htop                            # better interactive viewer (if installed)
```

## Process tree

```bash
pstree -p                       # tree view with PIDs
pstree -p $(pgrep nginx)        # tree for specific process
```

## Signals

```bash
kill -15 <pid>                  # SIGTERM (graceful shutdown)
kill -9 <pid>                   # SIGKILL (immediate, last resort)
kill -1 <pid>                   # SIGHUP (reload config, for many daemons)
killall nginx                   # kill all processes named nginx
pkill -f "python.*worker"       # kill by pattern match on full command
```

## File descriptors and sockets

```bash
lsof -p <pid>                   # all files open by process
lsof -i :8080                   # what's listening on port 8080
ss -tlnp                        # listening TCP ports with PID (faster than netstat)
```

## Resource limits

```bash
ulimit -n                       # current file descriptor limit
ulimit -n 65536                 # set for current shell session
cat /proc/<pid>/limits          # limits for running process
```

## /proc filesystem

```bash
cat /proc/<pid>/cmdline         # command line (null-delimited)
cat /proc/<pid>/environ         # environment (null-delimited)
ls -la /proc/<pid>/fd/          # open file descriptors
cat /proc/<pid>/net/tcp         # TCP connections
```

## systemd service management

```bash
systemctl status myservice
systemctl start/stop/restart myservice
systemctl enable/disable myservice
journalctl -u myservice -f      # follow service logs
```

Reference: https://man7.org/linux/man-pages/man1/ps.1.html
proc filesystem: https://man7.org/linux/man-pages/man5/proc.5.html
