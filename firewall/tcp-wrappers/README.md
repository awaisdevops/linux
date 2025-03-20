# TCP Wrappers Guide

TCP Wrappers is an application-level firewall/ACL that is configured to allow or deny network access from specific IPs or IP pools to a particular network service.

**Note:** In TCP Wrappers, "allow" takes precedence over "deny."

## Overview

TCP Wrappers works with network services using the `libwrap.so` library, which forwards requests to check the Access Control Lists (ACLs) in `/etc/hosts.allow` and `/etc/hosts.deny`. However, TCP Wrappers doesn’t work with all network services, as it is dependent on whether the service uses `libwrap.so`.

## How TCP Wrappers Work

1. **Allow/Deny Mechanism**: Requests are checked against the `/etc/hosts.allow` and `/etc/hosts.deny` files. If a service is allowed, the request is processed; if denied, the request is rejected.
   
2. **Order of Precedence**: The "allow" rules take precedence over the "deny" rules in TCP Wrappers.

## Checking Service Compatibility with TCP Wrappers

To determine whether a service can be controlled using TCP Wrappers, you can check if it uses the `libwrap.so` library. This can be done using the `ldd` command.

### Example: Checking vsftpd for libwrap.so

```bash
ldd /usr/sbin/vsftpd
```

Output:

```
libwrap.so.0 => /lib64/libwrap.so.0 (0x00007f20f716b000)
```

As `vsftpd` is linked with `libwrap.so`, it means that TCP Wrappers can be used for access control on this service.

### Example: Checking httpd for libwrap.so

```bash
ldd /usr/sbin/httpd | grep -i libwrap.so
```

Output:

No output means `httpd` does not use `libwrap.so`, and thus cannot be controlled using TCP Wrappers.

## Limitations of TCP Wrappers

TCP Wrappers is not considered highly secure because it passes the request directly to the service, where the ACLs are checked. If the request is already in the process of accessing the service, only then is it determined whether access is allowed or denied. 

**Important:** Kernel-level firewalls like `iptables` and `firewalld` prevent access to services by verifying requests *before* they enter the system. Therefore, they are generally more secure as they block unauthorized access at a much earlier stage.

## Types of Firewalls

1. **Hardware Firewalls**: These are commercial devices like Fortigate, Cisco ASA, etc.
2. **Software-Based Kernel-Level Firewalls**: These include `iptables` and `firewalld`.
3. **ISO Image Firewalls**: These are firewalls that run on bare-metal systems, functioning the same way as commercial hardware firewalls. Examples include `pfSense` and `IPCop`.
4. 

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
