# Network Monitoring Tools

This guide explains various network monitoring tools that can help with network management and troubleshooting. Below are the tools and commands you can use for effective network monitoring.

## Tools Overview

You can use the following tools to monitor your network:

1. **nmap**: Used for port scanning and security auditing of local/remote servers or entire networks.
2. **telnet**: Used to check the status of a port on local/remote servers.
3. **nc (netcat)**: Similar to telnet, used for checking port status and sharing data between client and server.
4. **netstat**: Displays network connections, routing tables, and network protocol statistics.
5. **ss**: A successor to `netstat` for displaying network statistics.
6. **traceroute**: Traces the full route from the source to the destination.
7. **tcpdump**: A packet sniffing and analysis tool.
8. **ping**: Checks if a server is up or not.

Other network monitoring tools include **Wireshark**, **iperf**, **iptraf**, and **nmon**.

---

## 1. nmap

`nmap` is used to check open ports on local/remote servers.

```bash
nmap 127.0.0.1
# Checking open ports locally on the local server
nmap 141.94.138.24
# Checking for open ports on a remote server
nmap 141.94.138.24 -p 22232
# Checking for open ports at the specified remote server and port number
nmap 192.168.247.0/24
# Finding open ports in the whole network
ip r l
# To find your network range
```

---

## 2. telnet

`telnet` checks for open ports on local/remote machines.

```bash
telnet 127.0.0.1 22
# Checking for open ports locally
telnet 127.0.0.1 80
# Checking for opened ports locally
nslookup rolustech.com
# Resolves the domain name to IP
telnet 104.198.1.23 80
# Checking for opened ports on a remote server
```

---

## 3. nc (Netcat)

`nc` (Netcat) is used to check open ports locally or remotely.

```bash
nc 127.0.0.1 22
# Checking if port 22 is open locally
nc -v 10.0.2.4 1234
# Checking if port 1234 is open remotely
nc -zv google.com 443
# Checking if port 443 is open remotely by domain
```

---

## 4. netstat

`netstat` displays network connections, routing tables, and various network statistics.

```bash
netstat -tulnp google.com
# Displaying port and connection status
netstat -tulnp google.com | grep -i 80
# Filter netstat results for port 80
```

---

## 5. ss (Socket Statistics)

`ss` is similar to `netstat`, used for showing network statistics.

---

## 6. ping

`ping` checks if a server is up or not.

```bash
ping -c 5 google.com
# Send 5 ICMP Echo requests
ping -s 1024 google.com
# Use 1024-byte ICMP Echo request
ping -i eth1 8.8.8.8
# Use a specified NIC interface (eth1)
```

---

## 7. traceroute

`traceroute` prints the hops/route to reach a server.

```bash
traceroute google.com
# Trace the route to google.com
```

---

## 8. tcpdump

`tcpdump` is a packet-sniffing tool that captures and analyzes network packets.

```bash
tcpdump -vvv
# Run tcpdump with maximum verbosity
```

---

## 9. iostat

`iostat` provides information about system I/O devices and partitions.

```bash
iostat
iostat -x -d -p
# Show partition resource usage
iostat -x -d -p /dev/sda
# Execute on a specific partition
iostat -x -d -p /dev/sda 2
# Display output every 2 seconds
```

---

## 10. pidstat

`pidstat` reports statistics based on process IDs (PIDs).

```bash
apt install sysstat
pidstat -p ALL
# Display CPU usage for all processes
pidstat -p 135
# Display CPU usage for process 135
pidstat -C mysql
# Display CPU usage for processes named 'mysql'
```

---

## 11. sar (System Activity Report)

`sar` is a powerful system performance monitoring tool. It reports system activity.

```bash
apt-get install sysstat
sar -u 2 30
# Displays CPU statistics every 2 seconds, 30 times
sar -r 1 4
# Monitor memory usage
sar -n DEV 1 5
# Monitor network activity
```

---

## 12. mpstat

`mpstat` shows CPU statistics, helping you track CPU usage per processor.

---

## 13. vmstat

`vmstat` is a performance monitoring tool used to check RAM, cache, buffer usage.

```bash
vmstat
# General output for system performance
vmstat -a
# Display active and inactive memory
vmstat -s
# Memory and scheduling statistics
vmstat -D
# Display disk statistics
```

---

## 14. strace

`strace` is used to trace system calls and signals for a specific process.

---

## License

This guide is licensed under the MIT License. You can freely use, modify, and distribute this guide. See the `LICENSE` file for more details.

This README provides a comprehensive overview of various network monitoring tools that can assist with managing and troubleshooting network-related issues. Each tool has specific commands and use cases to help you monitor your network performance and identify problems.
