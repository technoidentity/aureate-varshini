# Chapter 10 — Process Management

## 1. What is a Process?

- **Process** → An instance of a running program.
- **Multitasking** → Linux rapidly switches CPU time between processes.
- **PID** → Unique Process ID assigned by the kernel.
- **PPID** → Parent Process ID.
- **Parent process** → Creates child processes.
- **Daemon** → Background process providing system services.
- **systemd** → Manages system services and startup on modern Linux systems.

**Remember:** PID 1 is the system's initial userspace process (usually `systemd`).

## 2. Viewing Processes

| Command | Purpose |
|---|---|
| `ps` | Show processes |
| `ps aux` | Show processes from all users |
| `ps -ef` | Full process listing |
| `top` | Live process monitoring |
| `pgrep nginx` | Find process PID by name |
| `pstree` | Display parent-child hierarchy |
| `lsof` | List open files used by processes |

```bash id="cz82j9"
ps aux
top
pgrep nginx
pstree -p
```

**Remember:** `ps` → Snapshot, `top` → Live monitoring.

## 3. Process States

| State | Meaning |
|---|---|
| `R` | Running or runnable |
| `S` | Sleeping (interruptible) |
| `D` | Uninterruptible sleep (often I/O) |
| `T` | Stopped |
| `Z` | Zombie (finished but not reaped) |

Check process states:

```bash id="xqk7eu"
ps aux
```

The `STAT` column shows process states.

## 4. Foreground & Background Jobs

- **Foreground** → Process occupies the terminal.
- **Background** → Process runs while terminal remains available.

| Command | Purpose |
|---|---|
| `command &` | Start in background |
| `jobs` | List shell jobs |
| `Ctrl + Z` | Suspend foreground job |
| `bg` | Resume job in background |
| `fg` | Bring job to foreground |
| `Ctrl + C` | Interrupt foreground process |

```bash id="xv2ql4"
sleep 100 &
jobs
fg %1
```

`%1` → Job number 1.

**Remember:** Job ID and PID are different.

## 5. Killing Processes & Signals

`kill` sends a signal to a process.

| Command / Signal | Purpose |
|---|---|
| `kill PID` | Send SIGTERM (15) |
| `kill -9 PID` | Force kill (SIGKILL) |
| `kill -STOP PID` | Suspend process |
| `kill -CONT PID` | Resume process |
| `killall name` | Signal processes matching name |

```bash id="1v79kz"
kill 1234
kill -9 1234
killall firefox
```

**Important:** Use `kill PID` first. `kill -9` should be a last resort because the process cannot perform cleanup.

## 6. Process Priority — nice & renice

Linux uses scheduling priorities to manage CPU access.

- **Nice value** → Ranges from `-20` to `19`.
- Lower nice value → Higher scheduling priority.
- Higher nice value → Lower scheduling priority.

```bash id="5xqwel"
nice -n 10 command
renice -n 10 -p 1234
```

| Command | Purpose |
|---|---|
| `nice` | Start process with modified priority |
| `renice` | Modify priority of running process |

**Remember:** Higher nice value means the process is *nicer* to others.

## 7. nohup — Keep Processes Running

`nohup` prevents processes from terminating due to SIGHUP when a terminal disconnects.

```bash id="hsxm9a"
nohup ./script.sh > output.log 2>&1 &
```

- `nohup` → Ignore hangup signal.
- `> output.log` → Redirect output.
- `2>&1` → Redirect errors to stdout.
- `&` → Run in background.

**Remember:** `nohup` helps a process survive terminal disconnection; it does not restart crashed processes.

## 8. System Shutdown Commands

| Command | Purpose |
|---|---|
| `shutdown -h now` | Shut down system |
| `shutdown -r now` | Reboot system |
| `reboot` | Restart system |
| `poweroff` | Power off system |
| `halt` | Halt system |

Use these carefully, especially on shared machines or servers.

## 9. Quick Recall

- `PID` → Process ID.
- `PPID` → Parent Process ID.
- `ps aux` → All processes.
- `top` → Live processes.
- `pgrep` → Find PID by name.
- `pstree` → Process hierarchy.
- `lsof` → Open files.
- `jobs` → Shell background/stopped jobs.
- `Ctrl + Z` → Suspend.
- `bg` → Background.
- `fg` → Foreground.
- `kill` → Send signal.
- `SIGTERM (15)` → Request termination.
- `SIGKILL (9)` → Force termination.
- `nice` → Set initial priority.
- `renice` → Change running priority.
- `nohup` → Ignore hangup signal.
- `reboot` → Restart system.

**Core takeaway:** Understand how Linux creates and manages processes. Focus on `ps`, `top`, `jobs`, `bg`, `fg`, `kill`, signals, and process priorities for everyday troubleshooting.