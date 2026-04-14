# Linux process management — quick reference

Commands I use enough to want at hand but not enough to always remember.

## Finding processes

```bash
# List all processes
ps aux

# Find by name
ps aux | grep processname
pgrep processname          # cleaner, returns PIDs
pgrep -l processname       # include names

# Interactive process viewer
top
htop                       # better, if installed
```

## Signals

```bash
kill PID                   # SIGTERM (ask nicely to stop)
kill -9 PID                # SIGKILL (force stop)
kill -HUP PID              # SIGHUP (reload config for many daemons)
killall processname        # send signal to all matching processes
pkill processname          # like killall but matches partial names
```

## Background/foreground

```bash
command &                  # run in background
Ctrl+Z                     # suspend current job
bg %1                      # resume job 1 in background
fg %1                      # bring job 1 to foreground
jobs                       # list background jobs
disown %1                  # detach job from shell (survives terminal close)
nohup command &            # run ignoring hangup signal
```

## Process priority (niceness)

```bash
nice -n 10 command         # run with lower priority (10 = less priority, -20 = max)
renice 10 -p PID           # change priority of running process
```

## lsof and fuser

```bash
lsof -i :8080              # what's using port 8080
lsof -u username           # all files opened by user
fuser 8080/tcp             # PIDs using port 8080
fuser -k 8080/tcp          # kill processes using port
```

Linux process management manual: [https://man7.org/linux/man-pages/man1/ps.1.html](https://man7.org/linux/man-pages/man1/ps.1.html)
