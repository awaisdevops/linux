# Configuring IP Address in CentOS

This guide explains how to configure an IP address on a CentOS system, both automatically and manually.

---

## Configuring IP Address in CentOS 6

1. **Check IP Address Availability**:

   First, check if the IP address is available by pinging it from the base machine. It should return `"Destination host unreachable"`. Ensure the network ID of the IP address is the same as the base machine.

   ```bash
   ping 192.168.6.245
   ```

   If the result is `"Destination host unreachable"`, the IP is free and can be assigned.

2. **Automatic IP Configuration (Using Setup Tool)**:

   1. Run the `setup` tool:
      ```bash
      setup
      ```

   2. Navigate to **Network Configuration**, then **Device Configuration**, and select the **eth0** (Ethernet card) driver. Set it to **DHCP** (by pressing the space bar) or manually enter the IP details.
   
   3. Restart the network manager to apply the changes:
      ```bash
      service network restart
      ```

   4. Verify the IP configuration:
      ```bash
      ip r l
      ```

3. **Manual IP Configuration**:

   1. Use `ifconfig` to manually configure the IP:
      ```bash
      ifconfig eth0 192.168.6.250 netmask 255.255.255.0
      ```

   2. To verify gateway information, use the following:
      ```bash
      route -n
      ```

   3. Configure the gateway:
      ```bash
      route add default gw 192.168.6.10
      ```

   4. To remove the gateway:
      ```bash
      route del default gw 192.168.6.10
      ```

   5. To bring the interface up or down:
      ```bash
      ifconfig eth0 down
      ifconfig eth0 up
      # or
      ifup eth0
      ifdown eth0
      ```

   6. To persist IP address changes, edit the network interface configuration file:
      ```bash
      vim /etc/sysconfig/network-scripts/ifcfg-eth0
      ```

      Example content for `ifcfg-eth0`:
      ```ini
      DEVICE=eth0
      BOOTPROTO=static   # Use 'static' for manual configuration or 'dhcp' for auto configuration
      HWADDR=00:0c:29:f4:9c:87
      IPV6INIT=yes
      NM_CONTROLLED=no   # Disable network manager for CentOS 6
      ONBOOT=yes
      TYPE=Ethernet
      UUID="5d7e141b-64c1-4c1b-b272-f8cad18378e0"
      IPADDR=192.168.6.250
      NETMASK=255.255.255.0
      GATEWAY=192.168.6.10
      DNS1=8.8.8.8
      USERCTL=no
      PEERDNS=yes
      ```

   7. Restart the network:
      ```bash
      service network restart
      ```

   8. Confirm the IP configuration:
      ```bash
      ip r l
      ```

---

## Network Troubleshooting Steps

1. Ensure the base machine has an active internet connection (if using a hypervisor) and can ping servers by both IP and hostname.

2. For VirtualBox, set the network setting to **Bridge Adapter**; for VMware, set it to **NAT**.

3. Ensure that the Ethernet driver name and the network interface file (`ifcfg-eth0`) match.

4. Check the following in `/etc/sysconfig/network-scripts/ifcfg-eth0`:
   ```ini
   DEVICE=eth0
   BOOTPROTO=static   # Set to 'static' for manual configuration
   HWADDR=00:0c:29:f4:9c:87
   IPV6INIT=yes
   NM_CONTROLLED=no   # Disable network manager for CentOS 6
   ONBOOT=yes
   TYPE=Ethernet
   UUID="5d7e141b-64c1-4c1b-b272-f8cad18378e0"
   IPADDR=192.168.6.250
   NETMASK=255.255.255.0
   GATEWAY=192.168.6.10
   DNS1=8.8.8.8
   USERCTL=no
   PEERDNS=yes
   ```

5. Ensure the system can ping both by IP and hostname. If it can't ping by hostname, check the DNS entry in `/etc/resolv.conf`:
   ```bash
   vim /etc/resolv.conf
   ```

   Example content:
   ```
   nameserver 8.8.8.8
   ```

6. Restart the network service:
   ```bash
   service network restart
   ```

7. If the network still doesn't work, comment out the MAC address (`HWADDR`) in `/etc/sysconfig/network-scripts/ifcfg-eth0`.

8. Stop the NetworkManager and disable it:
   ```bash
   service NetworkManager stop
   chkconfig NetworkManager off
   ```

9. Edit `/etc/sysconfig/network-scripts/ifcfg-eth0` and ensure `NM_CONTROLLED=no` is set.

---

## Network Services in RHEL 6

RHEL 6 uses two network services:
1. **network**: Primary service for enabling networking.
2. **NetworkManager**: Secondary service (to be disabled in RHEL 6).

To stop and disable **NetworkManager**:
```bash
service NetworkManager stop
chkconfig NetworkManager off
```

Also, ensure the `/etc/sysconfig/network-scripts/ifcfg-eth0` file has `NM_CONTROLLED=no` set.

---

## Various IP Address Checking Commands

- `ifconfig`
- `ip r l`
- `ip addr`
- `ip addr list`
- `ip a`
- `ip a list`

---

## List of Public DNS Servers

- `8.8.8.8`
- `8.8.8.4`
- `8.8.4.4`
- `4.4.2.2`
- `4.2.2.2`

---

## Important Network Commands

- To view Ethernet card properties:
  ```bash
  ethtool eth0
  ```

- List peripheral components:
  ```bash
  lspci -vvv
  ```

- Get Ethernet card info:
  ```bash
  lspci | grep -i ethernet
  ```

- Get Audio card info:
  ```bash
  lspci | grep -i audio
  ```

---

## Configuring Network in CentOS 7

To configure the network in CentOS 7, we need to edit the network interface file (either `enp0s3` or `ens33`) as done previously.

1. Restart **NetworkManager**:
   ```bash
   systemctl restart NetworkManager
   systemctl restart network.service
   ```

2. To auto-configure the network using **nmcli** or **nmtui** (recommended):
   ```bash
   systemctl restart NetworkManager
   systemctl restart network.service
   ```

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```

---

This `README.md` file outlines the steps for configuring an IP address on CentOS systems, troubleshooting network issues, and configuring networks on CentOS 6 and CentOS 7. It also includes important commands for managing network interfaces and DNS servers.
