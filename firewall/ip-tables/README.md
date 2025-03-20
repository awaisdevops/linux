# IPTables Guide

## Overview

Upon starting `iptables`, the kernel table is loaded into RAM, and within this kernel table, there's a module called `netfilter` (`netfilter.ko` kernel object), which is the actual firewall. Both `iptables` and `firewalld` are just commands used to invoke the `netfilter` kernel module, which implements network traffic rules.

Firewalls work on the principle of FIFO (First In First Out), meaning the rule written first will be implemented first and affect network traffic.

### Firewall States

There are several states in firewalls:
1. **ESTABLISHED**
2. **NEW**
3. **RELATED**

It's essential to allow established connections for the OUTPUT chain in firewalls; otherwise, the connections will be dropped.

---

## IPTables Default Policy

By default, `iptables` has an **ACCEPT** policy for all three chains (INPUT, OUTPUT, and FORWARD). This means that by default, it allows all incoming and outgoing traffic to the server.

To check the current policy:

```bash
iptables -L
```

To change the default policy from ACCEPT to DROP:

```bash
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP
```

This will block all incoming, outgoing, and forwarded traffic. You can adjust the policy for any chain as needed.

---

## Deleting a Rule

To delete a specific rule, use the following command:

```bash
iptables -D OUTPUT 2
```

This will delete the rule at the second position in the OUTPUT chain. Remember to specify the chain and rule number.

---

## Types of IPTables Tables

`iptables` has four types of tables, each serving a specific function:

1. **RAW Table**: Used for tracking and non-tracking of packets.
2. **MANGLE Table**: Used for packet modification.
3. **NAT Table**: Used for Network Address Translation.
4. **Filter Table**: The default `iptables` table used for filtering server traffic.

To list the rules for each table:

```bash
iptables -t raw -L
iptables -t mangle -L
iptables -t nat -L
iptables -t filter -L
```

---

## IPTables Service Commands

To check the status, restart, and configure `iptables` to start on boot:

```bash
service iptables status
service iptables restart
chkconfig iptables on
```

To view the current rules configured:

```bash
iptables -L
```

To flush all the rules:

```bash
iptables -F
```

---

## Example: Blocking and Allowing Traffic

### Block All Incoming Traffic

To block all incoming traffic, use:

```bash
iptables -A INPUT -s 0/0 -j REJECT
```

This rule will reject all inbound traffic.

### Block All Outgoing Traffic

To block all outgoing traffic:

```bash
iptables -A OUTPUT -d 0/0 -j REJECT
```

### Allow Incoming Traffic from a Specific IP

To allow a specific IP address (e.g., 192.168.3.136) to send traffic to your server:

```bash
iptables -A INPUT -s 192.168.3.136 -j ACCEPT
```

To save all `iptables` rules to hard disk:

```bash
service iptables save
```

The rules will be saved at `/etc/sysconfig/iptables`.

---

## Advanced Rules Example

### Block an IP and Allow a Network

To block a specific IP (e.g., 192.168.3.139) and allow a network (e.g., 172.16.0.0/16), you can add rules in the following order:

```bash
iptables -A INPUT -s 192.168.3.139 -j REJECT
iptables -A INPUT -s 172.16.0.0/16 -j ACCEPT
iptables -A INPUT -s 0/0 -j REJECT
```

### Changing the Default Policy to DROP

To change the default policy from ACCEPT to DROP for all chains:

```bash
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP
```

This will block all incoming, outgoing, and forwarded traffic unless explicitly allowed by other rules.

---

## Allowing Ports in IPTables

When a client sends a request to a server, the request is carried by an IP packet, which encapsulates data, source and destination IP addresses, and the port to connect to the server.

### Allowing Port 80 (HTTP)

To allow a specific IP (e.g., 192.168.8.18) to access port 80 (HTTP) on your server:

```bash
iptables -A INPUT -s 192.168.8.18/32 -p tcp --dport 80 -j ACCEPT
```

To allow established connections for the OUTPUT chain:

```bash
iptables -A OUTPUT -d 192.168.3.139/32 -m state --state ESTABLISHED -j ACCEPT
```

To confirm the rules:

```bash
iptables -L
```

### Allowing Port 22 (SSH)

To allow a specific IP (e.g., 192.168.3.139) to access port 22 (SSH) on your server:

```bash
iptables -A INPUT -s 192.168.3.139/32 -p tcp --dport 22 -j ACCEPT
iptables -A OUTPUT -d 192.168.3.139/32 -m state --state ESTABLISHED -j ACCEPT
```

You can test the connection with:

```bash
ssh admin2@server
```

### Allowing Ping

To allow ICMP (ping) traffic:

```bash
iptables -A INPUT -s 0/0 -p icmp --icmp-type 8 -j ACCEPT
iptables -A OUTPUT -d 0/0 -p icmp --icmp-type 0 -j ACCEPT
```

The ICMP echo request uses type 8, and the echo reply uses type 0.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```

### Notes:
- The license section uses the MIT License, but you can adjust it according to your preferred licensing terms (e.g., GPL, Apache).
- This `README.md` is designed to be clear and easy to follow for anyone learning about `iptables`. It includes example commands, explanations, and steps for configuring `iptables`.
