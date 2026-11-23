# Chapter 12 — How Linux Works: Processes and Resource Utilization

## 1. Processes & System Resources

- **Process** → Running instance of a program.
- **Kernel** → Manages processes and allocates system resources.
- **CPU** → Executes instructions.
- **Memory (RAM)** → Stores active program data.
- **I/O** → Communication with disks, networks, and devices.
- **System calls** → Requests made by processes to the kernel.

**Remember:** Processes compete for CPU, memory, and I/O resources. The kernel manages their allocation.

## 2. Monitoring Processes — `ps`, `top`

| Command | Purpose |
|---|---|
| `ps aux` | Snapshot of running processes |
| `top` | Live CPU and memory monitoring |
| `htop` | Interactive process monitor |
| `btop` | Modern resource monitor |

**Important `top` shortcuts:**

| Key | Action |
|---|---|
| `P` | Sort by CPU usage |
| `M` | Sort by memory usage |
| `T` | Sort by cumulative CPU time |
| `u` | Filter by user |
| `Space` | Refresh display |
| `q` | Quit |

**Remember:** `ps` gives a snapshot; `top` updates continuously.

## 3. Finding Open Files — `lsof`

- **`lsof`** → Lists open files and the processes using them.
- Includes regular files, network sockets, pipes, and libraries.

```bash
lsof -p 1234       # Open files of process 1234
lsof -i :8080      # Find process using port 8080
lsof /path/file    # Find processes using a file
```

**Useful for:** Debugging port conflicts and identifying processes holding files open.

## 4. Process Tracing — `strace`

- **`strace`** → Traces system calls made by processes.
- Useful for investigating file access, permission errors, and unexpected program behavior.

```bash
strace ls
strace -p 1234
strace -e trace=file ls
```

**Remember:** `strace` helps understand how a program interacts with the kernel.

## 5. CPU & Memory Utilization

| Metric | Meaning |
|---|---|
| `%CPU` | CPU usage |
| `%MEM` | Percentage of physical memory used |
| `RES` / `RSS` | Resident physical memory |
| `VIRT` | Virtual address space |
| Load average | Average runnable and uninterruptibly waiting tasks |

```bash
free -h       # Check memory usage
uptime        # Check system load average
top           # Monitor CPU and memory
```

**Remember:** High load average does not always mean high CPU usage; I/O waiting can contribute.

## 6. Quick Recall

- `ps` → Process snapshot.
- `top` → Live process monitoring.
- `htop` / `btop` → Interactive resource monitoring.
- `lsof` → Open files and ports.
- `strace` → Trace system calls.
- `free -h` → Memory usage.
- `uptime` → System load.
- CPU, memory, I/O → Main hardware resources.
- Kernel → Allocates resources and manages processes.

**Core takeaway:** Understand how Linux manages processes and system resources. Focus on `top`, `lsof`, and `strace` for troubleshooting performance, open files, and process behavior.