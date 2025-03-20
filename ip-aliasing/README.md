# IP Aliasing Guide

IP aliasing is the process of assigning multiple IP addresses to a single network interface. This technique is useful for setting up multiple virtual sites on a web server using one network interface, each with a different IP address on the same subnet.

You can assign up to 256 IP addresses to a single network interface.

---

## What is IP Aliasing?

IP aliasing involves associating more than one IP address to a single network interface. Since IP addresses are managed by the network interface card (NIC), to assign a new IP address, you'll need to create a virtual interface or alias of the original NIC driver.

For example:
```bash
ifconfig eth0:0 192.168.6.251 netmask 255.255.255.0
ifconfig eth0:1 192.168.6.252 netmask 255.255.255.0
```

To confirm, you can use the following command:
```bash
ifconfig -a
```

The output will show something like this:
```
eth0:0    Link encap:Ethernet  HWaddr 00:0C:29:F4:9C:87  
          inet addr:192.168.6.251  Bcast:192.168.6.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
```

---

## Recommended Way of Creating IP Aliases

The recommended way to configure multiple IP addresses on a single network interface involves creating virtual interfaces (`eth0:0`, `eth0:1`, etc.). Here’s how to do it:

1. **Create Multiple IP Addresses**:

   We will configure three IP addresses: `192.168.6.251`, `192.168.6.252`, and `192.168.6.253` on the server’s network interface `eth0`. We will create three virtual interfaces: `eth0:0`, `eth0:1`, and `eth0:2`.

   - First, navigate to the network scripts directory:
     ```bash
     cd /etc/sysconfig/network-scripts/
     ```

   - Copy the existing `ifcfg-eth0` file to create new configurations for each virtual interface:
     ```bash
     cp ifcfg-eth0 ifcfg-eth0:0
     cp ifcfg-eth0 ifcfg-eth0:1
     cp ifcfg-eth0 ifcfg-eth0:2
     ```

   - Edit each virtual interface configuration file:

     1. **Edit `ifcfg-eth0:0`** to set the IP address `192.168.6.251`:
        ```bash
        vim ifcfg-eth0:0
        ```
        Change the NIC driver name to `eth0:0` and IP address to `192.168.6.251`.

     2. **Edit `ifcfg-eth0:1`** to set the IP address `192.168.6.252`:
        ```bash
        vim ifcfg-eth0:1
        ```
        Change the NIC driver name to `eth0:1` and IP address to `192.168.6.252`.

     3. **Edit `ifcfg-eth0:2`** to set the IP address `192.168.6.253`:
        ```bash
        vim ifcfg-eth0:2
        ```
        Change the NIC driver name to `eth0:2` and IP address to `192.168.6.253`.

   - Restart the network service to apply the changes:
     ```bash
     service network restart
     ```

   - Verify the configuration:
     ```bash
     ifconfig -a
     ```

---

## Assigning a Range of Multiple IP Addresses

You can also assign a range of IP addresses to a network interface. Here’s how:

1. **Create the IP Range Configuration**:

   - Navigate to the network scripts directory:
     ```bash
     cd /etc/sysconfig/network-scripts/
     ```

   - Copy the `ifcfg-eth0` file to create a new configuration for the IP range:
     ```bash
     cp ifcfg-eth0 ifcfg-eth0-range0
     ```

   - Edit the `ifcfg-eth0-range0` file:
     ```bash
     vim ifcfg-eth0-range0
     ```

   - Add the following entries to define the start and end of the IP range:
     ```ini
     IPADDR_START=192.168.6.240
     IPADDR_END=192.168.6.249
     ```

2. **Restart the Network**:
   ```bash
   service network restart
   ```

3. **Verify the IP Alias**:
   ```bash
   ifconfig -a
   ```

---

## Conclusion

IP aliasing is a powerful feature that allows you to configure multiple IP addresses on a single network interface. This can be useful for virtual hosting, creating multiple virtual sites, or organizing different services on a single physical server.

By following this guide, you can easily configure IP aliases on CentOS or similar Linux distributions.

---

## References

- [Create Multiple IP Addresses on One Network Interface - Tecmint](https://www.tecmint.com/create-multiple-ip-addresses-to-one-single-network-interface/)
```

---

This `README.md` file explains the process of IP aliasing on a Linux system, provides examples of configuring multiple IP addresses, and offers troubleshooting steps to verify the configuration.
