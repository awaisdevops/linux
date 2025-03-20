# DNS Server Setup Guide

This guide explains how to set up two types of DNS configurations on CentOS:

1. **DNS Caching + Recursive Server**
2. **Land DNS Server**

## 1. DNS Caching + Recursive Server

### 1.1 Install DNS Server (BIND)

```bash
yum install bind-* -y
```

The DNS server is installed under the name `bind` in CentOS. The BIND service runs on port 53.

### 1.2 DNS Server Configuration Files

There are two configuration files for the BIND package/DNS server:

- `/etc/named.conf`
- `/var/named/chroot/etc/named.conf`

The actual configuration file used is located in the DNS home directory (`/var/named/chroot`).

```bash
mv /etc/named.conf /opt/
```

Move the DNS configuration file from `/etc/` to another directory before starting the DNS service. This is because we'll use the DNS-related files from the root directory (`/var/named/chroot`).

The default `/var/named/chroot/etc/named.conf` file is empty:

```bash
echo " " > /var/named/chroot/etc/named.conf
```

This clears any previous configuration.

### 1.3 Start DNS Service

To start the DNS service:

```bash
systemctl restart named-chroot
systemctl enable named-chroot
systemctl status named-chroot
```

### 1.4 Check DNS Port

Check the DNS port status:

```bash
netstat -tulnp | grep -i 53
```

Test if the DNS port is up:

```bash
telnet 127.0.0.1 53
```

Check the DNS process:

```bash
ps -el | grep -i named
```

### 1.5 Allow DNS Service in Firewall

To allow DNS traffic through the firewall:

```bash
firewall-cmd --permanent --add-service=dns
firewall-cmd --reload
firewall-cmd --list-services
```

This adds the DNS service, reloads the firewall, and checks if the service is available.

### 1.6 Test the DNS Server

Now that we've set up a caching + recursive DNS server, you can test it by adding the DNS server's IP address to your `/etc/resolv.conf` file on the client machine:

```bash
nameserver 192.168.3.139
```

Test the DNS resolution using `dig`:

```bash
dig @192.168.3.139 youtube.com
```

Since it's a recursive and caching server, the first time it resolves the domain name, it will take some time, but subsequent requests will be faster due to caching.

```bash
dig youtube.com
```

The second `dig` query should be much faster, as the result is cached.

---

## 2. Land DNS Server

A **Land DNS Server** is used in a scenario where the server should not go outside for name-to-IP resolution, even when using your own configured DNS server. This server resolves domain names locally.

### 2.1 Create a Zone for Land DNS Server

To configure a Land DNS Server, create a zone and a zone file. For example, create a zone for `cnn.com`.

```bash
vim /var/named/chroot/etc/named.conf
```

Add the following entry:

```bash
zone "cnn.com" {
    type master;
    file /var/named/cnn.com;
}
```

### 2.2 Create the Zone File

Navigate to the directory where the zone files are stored:

```bash
cd /var/named/chroot/var/named/
```

Create the zone file for `cnn.com`:

```bash
touch cnn.db
```

Copy a sample zone file to the new zone file:

```bash
cat /usr/share/doc/bind-9.11.4/sample/var/named/named.localhost > /var/named/chroot/var/named/cnn.db
```

Edit the zone file:

```bash
vim cnn.db
```

Add the domain name and IP address:

```bash
cnn.com   IN   A   151.101.3.5
```

### 2.3 Restart DNS Server

After adding the zone file, restart the DNS server:

```bash
systemctl restart named-chroot
```

### 2.4 Test the Land DNS Server

Test the DNS resolution for `cnn.com`:

```bash
dig @192.168.3.139 cnn.com
```

The first time the domain name is resolved, it will be cached and the resolution time should be quick.

---

### Breakdown:

- **Section 1**: This is dedicated to setting up the **DNS Caching + Recursive Server**. It includes installation, configuration, and testing.
- **Section 2**: This is dedicated to setting up the **Land DNS Server**. It covers the process of creating a local DNS resolution for specific domains.
