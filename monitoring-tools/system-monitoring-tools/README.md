# System Monitoring Tools Guide

## Process and Daemons or Performance Monitoring

We monitor the performance of server resources like CPU, hard disk, RAM, and network, which is why performance monitoring is critical.

Every process in RAM has a Process ID (PID). PIDs are assigned randomly by a program called `erandom`. Roughly 20% of processes have fixed PIDs, while the remaining 80% have dynamically assigned PIDs. Up to CentOS 6, `init` was the father of all processes, but starting from CentOS 7/8, `systemd` has taken over this role.

When you run a command like `df -h`, its process is loaded into RAM, performs the task, and then terminates. This is a simple process. However, the process of a service, known as a daemon, runs indefinitely until manually closed (e.g., `httpd`, `mysqld`). Daemons come in two types:
1. **Application daemon**: Killing the process of application daemons has no effect on the system.
2. **System daemon**: Killing the process of a system daemon may crash the system (e.g., `init`).

### State of Processes in Linux

- **R**: Running
- **S**: Sleeping (the process is in RAM but not being processed)
- **Zombie/Defunct**: A dead process waiting for its parent to terminate. Zombie processes can be cleared by rebooting the server, typically after taking downtime. These processes often result from flawed application or database designs.

---

## Performance Monitoring Tools

Linux offers a variety of tools to monitor system performance, including CPU, hard disk, RAM, and network:

1. `top`
2. `ps`
3. `pstree`
4. `lsof`
5. `pidof`
6. `pgrep`
7. `htop`
8. `sar`
9. `iostat` (to check hard disk load)
10. `vmstat` (to check CPU, RAM, and swap load)
11. `iometer`
12. `ipref` (to check network bandwidth)
13. `tcpdump` (similar to Wireshark, used for network monitoring)
14. `sar`
15. `pidstat`
16. `strace` (shows system calls and processes triggered by a command)
17. `ipcs`
18. `lsof`
19. `htop`
20. `nmon`

---

## Commands to Check Processes and Server Resource Load

### Load on Server Resources

Load refers to the number of applications waiting in the queue to be processed by the CPU.

#### `top` Command
`top` is similar to the Windows Task Manager. It displays aggregated load across server resources horizontally and the load of each process vertically. Kernel manages process information via the `/proc/` directory, and `top` fetches data from there.

Example to store output:
```bash
top -n 1 > /opt/resource_statistics
```

#### Aggregated Load on Server Resources:
```bash
top - 13:20:15 up  2:04,  2 users,  load average: 0.03, 0.01, 0.00
Tasks: 149 total,   1 running, 148 sleeping,   0 stopped,   0 zombie
```

- **1.1%us**: CPU usage by the user
- **96.9%id**: Idle CPU value
- **0.1%wa**: CPU waiting for an I/O cycle
- **0.1%hi**: Hardware interrupt (indicates hardware problems)
- **0.1%si**: Software interrupt (indicates software issues)
- **0.0%st**: Software tracing

#### Memory and Swap Information:
```bash
Mem: 1003020k total, 828524k used, 174496k free, 25028k buffers
Swap: 1572860k total, 8k used, 1572852k free, 458824k cached
```

---

## 15 Simple `top` Command Examples on Linux to Monitor Processes

For more information on `top` command usage, see [15 Simple Top Command Examples on Linux](https://www.binarytides.com/linux-top-command/).

**Common Flags for `top` Command**:
- `P`: Sort processes by CPU usage.
- `N`: Sort by Process ID.
- `T`: Sort by running time.
- `R`: Reverse the sorting order.
- `b`: Highlight the sorted column background.
- `x`: Highlight the sorted column with bold text.
- `d`: Change the update delay.
- `o`: Filter processes by command name or CPU usage.

Example to display all CPU core statistics:
```bash
top -d 1.0 -b | grep cpu
```

---

## Checking Server Uptime

You can check the uptime of the server using the following commands:
```bash
top
uptime
cat /proc/uptime
```

---

## Killing Processes

To terminate processes, use the `kill` command with the PID or `pkill` with the program name. By default, `kill` uses signal 15 (SIGTERM) to terminate processes gracefully, but signal 9 (SIGKILL) can be used to forcefully kill a process.

Examples:
```bash
kill -l  # List all signals
kill <PID>  # Kill a process by PID
pkill firefox  # Kill all processes named 'firefox'
pkill -9 firefox  # Forcefully kill 'firefox' processes
```

---

## Fork Bombing / Load Testing

A fork bomb script can overload system resources by creating infinite loops of processes:
```bash
:() { :|:& };:
```

---

## General Steps for Performance Monitoring

### Case #1: High Server Utilization
1. Check CPU idle value using `top`. If it's 0%, the CPU is fully utilized.
2. Check RAM and swap usage.
3. Identify the process causing high resource usage.
4. Engage the relevant team (e.g., Java app team) to optimize the application.

### Case #2: No Specific Process Found
If no process is identified, check the disk and network performance:
- Use `df -h` to check disk space.
- Use `iostat` and `iometer` to monitor disk performance.
- Use `tcpdump`, `wireshark`, `ipref`, `nmon`, or `sar` to monitor network load.

### Case #3: Check for Memory Leaks
Use `valgrind` to check for memory leaks and clear cache and buffers using:
```bash
sync; echo 3 > /proc/sys/vm/drop_caches
```

### Case #4: Hardware Issues
If no software-related issues are found, check for hardware problems:
- Use management consoles for component integrity checks.
- Use `memtest` for RAM checks.
- Use `i7z` for overall hardware detection.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
