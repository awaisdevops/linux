# Network Monitoring Tools Guide

This document provides an overview of various network monitoring tools used to monitor and troubleshoot network and server performance. These tools allow you to check the status of ports, monitor network traffic, analyze packet data, and diagnose server resource utilization issues.

## Table of Contents

1. [Network Monitoring Tools](#network-monitoring-tools)
   - [1. nmap](#1-nmap)
   - [2. telnet](#2-telnet)
   - [3. nc](#3-nc)
   - [4. netstat](#4-netstat)
   - [5. ss (Socket Statistics)](#5-ss-socket-statistics)
   - [6. traceroute](#6-traceroute)
   - [7. tcpdump](#7-tcpdump)
   - [8. ping](#8-ping)
   - [9. Other Tools](#9-other-tools)
   
2. [Localhost](#localhost)

3. [Performance Monitoring](#performance-monitoring)
   - [Case 1: Diagnosing High CPU and RAM Usage](#case-1-diagnosing-high-cpu-and-ram-usage)
   - [Case 2: Hard Disk and Network Utilization](#case-2-hard-disk-and-network-utilization)
   - [Case 3: Checking for Memory Leaks](#case-3-checking-for-memory-leaks)
   - [Case 4: Hardware Fault Diagnosis](#case-4-hardware-fault-diagnosis)
   
4. [Additional Monitoring Tools](#additional-monitoring-tools)
   - [iostat](#iostat)
   - [pidstat](#pidstat)
   - [sar](#sar)
   - [mpstat](#mpstat)
   - [vmstat](#vmstat)
   - [strace](#strace)

5. [License](#license)

---

## Network Monitoring Tools

### 1. nmap
`nmap` is a popular network scanning tool used to check for open ports on local or remote servers and perform security audits.

**Examples:**
```bash
nmap 127.0.0.1          # Checking open ports locally
nmap 141.94.138.24      # Checking open ports on a remote server
nmap 141.94.138.24 -p 22232   # Checking specific port on a remote server
nmap 192.168.247.0/24   # Checking open ports across a network range
```

### 2. telnet
`telnet` is a simple tool used to check the status of open ports on a local or remote machine.

**Examples:**
```bash
telnet 127.0.0.1 22     # Checking port 22 locally
telnet 104.198.1.23 443  # Checking port 443 on a remote server
```

### 3. nc (netcat)
`nc` (netcat) is similar to `telnet` but also allows for data sharing between a client and server.

**Examples:**
```bash
nc 127.0.0.1 22         # Checking port 22 locally
nc -v 10.0.2.4 1234      # Checking remote port 1234
nc -zv google.com 443    # Checking port 443 on google.com
```

### 4. netstat
`netstat` is a command-line tool that displays active network connections and routing information.

**Examples:**
```bash
netstat -tulnp google.com     # Checking active connections on google.com
netstat -tulnp google.com | grep -i 80   # Filtering results for port 80
```

### 5. ss (Socket Statistics)
`ss` is a faster, more efficient tool for showing network statistics and socket connections. It’s a successor to `netstat`.

**Example:**
```bash
ss -tulnp   # Display socket statistics
```

### 6. traceroute
`traceroute` is used to trace the full route from the source to the destination server.

**Example:**
```bash
traceroute google.com
```

### 7. tcpdump
`tcpdump` is a packet sniffing and analysis tool that captures network traffic for inspection.

**Example:**
```bash
tcpdump -vvv    # Capturing detailed network traffic
```

### 8. ping
`ping` is used to check if a server is reachable (i.e., if it is up and responding to requests).

**Examples:**
```bash
ping -c 5 google.com   # Sending 5 ICMP echo requests
ping -i eth1 8.8.8.8   # Specifying NIC interface
```

### 9. Other Tools
Other useful network monitoring tools include:
- `Wireshark`: A network protocol analyzer.
- `iperf`: A tool for measuring network bandwidth performance.
- `iptraf`: A real-time network statistics monitoring tool.
- `nmon`: A performance monitoring tool for Linux.

---

## Localhost

**127.0.0.1** is the loopback address, commonly referred to as `localhost`. It’s used for internal testing on your machine without interacting with the network.

---

## Performance Monitoring

### Case 1: Diagnosing High CPU and RAM Usage
If a server’s CPU or RAM is at high utilization, you should first check the CPU idle value using the `top` command. If the CPU idle value is 0%, then the load is at 100%. After this, check the RAM usage and swap status to identify which processes are consuming resources.

**Key steps:**
1. Check CPU usage and identify processes.
2. Check RAM and swap space.
3. Engage relevant teams before killing processes.

### Case 2: Hard Disk and Network Utilization
If high server utilization is not caused by CPU or RAM, check hard disk integrity and network utilization.

- Use tools like `iostat` and `iometer` for disk performance.
- Monitor network load with tools like `tcpdump`, `wireshark`, and `iptraf`.

### Case 3: Checking for Memory Leaks
Memory leaks may cause abnormal resource utilization. Clear cache and buffer memory using the following command:
```bash
sync; echo 3 > /proc/sys/vm/drop_caches
```

- **Buffer**: Temporary storage for data moving between locations.
- **Cache**: Stores frequently used data for quicker access.

### Case 4: Hardware Fault Diagnosis
If no software issues are identified, check for faulty hardware components using management consoles or tools like `memtest` for RAM and `i7z` for overall system integrity.

---

## Additional Monitoring Tools

### iostat
`iostat` provides detailed statistics about system input/output devices and performance.

**Examples:**
```bash
iostat -x -d -p /dev/sda 2 3  # Monitor disk performance every 2 seconds
```

### pidstat
`pidstat` provides statistics for individual processes, helping debug performance issues.

**Examples:**
```bash
pidstat -p 135      # Monitor CPU usage for process with ID 135
pidstat -C mysql    # Monitor CPU usage for MySQL process
```

### sar
`sar` (System Activity Report) is used for overall system performance monitoring.

**Examples:**
```bash
sar -u 2 30       # Monitor CPU usage every 2 seconds for 30 times
sar -n DEV 1 5    # Monitor network activity
```

### mpstat
`mpstat` provides CPU statistics, showing resource utilization per processor.

**Example:**
```bash
mpstat -P ALL 1    # Monitor all CPUs' usage every second
```

### vmstat
`vmstat` is used to check overall system performance, including memory, swap, and CPU statistics.

**Example:**
```bash
vmstat 2 5   # Display statistics every 2 seconds for 5 times
```

### strace
`strace` traces system calls and signals for debugging and monitoring.

---

## License

MIT License. See the [LICENSE](LICENSE) file for more details.
