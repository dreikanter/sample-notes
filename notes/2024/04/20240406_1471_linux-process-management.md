# Linux process management reference

## Viewing processes

```bash
ps aux                    # all processes, BSD format
ps aux | grep nginx       # filter by name
ps -ef                    # all processes, full format
pgrep nginx               # just the PIDs
pgrep -l nginx            # PIDs with names

top                       # interactive process viewer
htop                      # better interactive viewer
```

## Signals

```bash
kill <pid>              # send SIGTERM (graceful shutdown request)
kill -9 <pid>           # send SIGKILL (force kill, cannot be ignored)
kill -HUP <pid>         # SIGHUP — reload config on many daemons
killall nginx           # kill all processes named nginx
pkill -f "python app"   # kill by pattern match against full command
```

Common signals:
- `1`  SIGHUP — hangup / reload
- `2`  SIGINT — interrupt (Ctrl+C)
- `15` SIGTERM — terminate (default)
- `9`  SIGKILL — force kill (unblockable)
- `19` SIGSTOP — pause (Ctrl+Z)
- `18` SIGCONT — continue

## Background jobs

```bash
command &               # run in background
Ctrl+Z                  # suspend current process
bg                      # continue suspended in background
fg                      # bring background job to foreground
fg %2                   # bring job 2 to foreground
jobs                    # list background jobs
```

## nohup and disown

```bash
nohup command &         # keep running after logout
disown %1               # remove job from shell's job table
```

## Process priority

```bash
nice -n 10 command      # run with lower priority (nice value 10)
renice 15 -p <pid>      # change running process priority
```

Nice values: -20 (highest priority) to 19 (lowest). Only root can go negative.

## lsof and fuser

```bash
lsof -p <pid>           # files opened by process
lsof -i :8080           # process using port 8080
fuser 8080/tcp          # same, simpler output
lsof -u username        # files opened by user
```

Reference: https://man7.org/linux/man-pages/man1/ps.1.html
