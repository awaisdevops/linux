# Performance Monitoring in Linux

Monitoring the performance of key system resources such as CPU, hard disk, RAM, and network is an essential aspect of system administration. This guide explains how to monitor these resources in Linux using various performance monitoring tools and techniques.

## Processes and Daemons

In Linux, everything running in RAM is a process, and each process is assigned a unique **PID (Process ID)**. The **PID** is assigned randomly, with roughly 20% of programs having fixed PIDs and the remaining 80% having dynamic PIDs. 

- In CentOS 6, **init** was the parent of all processes.
- In CentOS 7 and 8, **systemd** serves as the parent of all processes.

A simple command like `df -h` runs as a process in RAM and gets executed before being terminated. A **daemon**, on the other hand, is a special type of process that runs continuously after it is started (e.g., `httpd`, `mysqld`). Daemons are classified into two types:

1. **Application Daemon**: Terminating an application daemon has no impact on the system.
2. **System Daemon**: Killing a system daemon process, like `init`, may crash the entire system.

### Process States in Linux

- **R**: Running
- **S**: Sleeping (process is in passive mode, in RAM but not being processed)
- **Z**: Zombie/Uninterruptable/Defunct (a dead process residing in RAM waiting for the parent process to be terminated)

Zombie processes are often the result of flawed application or database design. If they cause performance issues, the server must be rebooted after taking downtime.

---

## Performance Monitoring Tools

Linux provides various tools to monitor the performance of server resources like CPU, hard disk, RAM, and network. Some of the commonly used performance monitoring tools are:

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
16. `strace` (shows system calls made by commands, identifying backend processes)
17. `ipcs`
18. `lsof`
19. `htop`
20. `nmon`

---

## Commands to Check Processes and Server Load

### Load on Server Resources

In Linux, "load" refers to the number of processes waiting to be processed by the CPU. You can check the load with several commands:

#### `top`

- Equivalent to Windows Task Manager, it shows aggregated load on server resources in a horizontal layout and individual process load in a vertical layout.
- The `top` command fetches process data from `/proc/` directory, which is cleared upon reboot.

```bash
top
```

#### Example: Storing Top Command Output

```bash
top -n 1 > /opt/resources_statistics
```

#### Understanding the Top Command Output

Example output from `top`:

```
top - 13:20:15 up  2:04,  2 users,  load average: 0.03, 0.01, 0.00
Tasks: 149 total,   1 running, 148 sleeping, 0 stopped, 0 zombie
%Cpu(s):  1.1 us,  1.5 sy,  0.2 ni, 96.9 id,  0.1 wa,  0.1 hi,  0.1 si,  0.0 st
```

- **1.1%us**: CPU usage by user processes.
- **1.5%sy**: CPU usage by system processes.
- **96.9%id**: Idle CPU time.
- **0.1%wa**: Time waiting for I/O cycles.
- **0.1%hi**: Hardware interrupt time.
- **0.1%si**: Software interrupt time.

Example memory usage output:

```
Mem:   1003020k total,   828524k used,   174496k free,    25028k buffers
Swap:  1572860k total,        8k used,  1572852k free,   458824k cached
```

#### Checking RAM and Swap Usage

```bash
free -m
```

### Other Useful Commands

- **`uptime`**: Shows how long the server has been running.
- **`cat /proc/uptime`**: Displays server uptime.
- **`ps`**: Lists running processes.

Example:

```bash
ps -aux | less
ps -ef | grep -i firefox
```

---

## Simple `top` Command Examples

Here are some examples of using the `top` command to monitor processes:

- **Sort by CPU usage**: Press `P`
- **Sort by process ID**: Press `N`
- **Sort by running time**: Press `T`
- **Reverse the sorting order**: Press `R`
- **Highlight sorted column**: Press `b` for background or `x` for bold text.
- **Change update delay**: Press `d` and specify the interval (default is 3 seconds).
- **Filter processes**: Press `o` or `O` and use filters (e.g., `COMMAND=apache`).

Example to display processes of a user:

```bash
top -u username
```

---

## Killing Processes

To terminate processes in Linux, we use the `kill` or `pkill` commands:

- **`kill <pid>`**: Terminates a process by its PID.
- **`pkill <process-name>`**: Terminates a process by its name.
- **`kill -9 <pid>`**: Forcefully terminates a process.

### Common Kill Signals

- **SIGTERM (15)**: Graceful termination (default signal).
- **SIGKILL (9)**: Forceful termination.

Example:

```bash
kill 5364
pkill firefox
pkill -9 firefox
```

---

## Fork Bomb (Load Testing)

A **fork bomb** is a denial-of-service technique that creates a large number of processes to consume system resources, effectively bringing the system down.

Example script for a fork bomb:

```bash
:() { :|:& };:
```

This script will rapidly spawn processes, consuming all system resources.

---

## Conclusion

By using these performance monitoring tools and commands, you can effectively manage and monitor server resources in Linux. Whether you're checking the CPU usage, memory consumption, or process management, these tools will help ensure your system is performing optimally.
