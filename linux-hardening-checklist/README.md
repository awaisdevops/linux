# Linux Hardening Checklist

This checklist provides a comprehensive guide for hardening a Linux system, ensuring that the operating system is secured against potential attacks. It includes steps for system installation, patching, OS hardening, user access management, network security, and securing specific services such as Apache.

## Table of Contents

1. [System Installation & Patching](#system-installation--patching)
2. [OS Hardening](#os-hardening)
3. [User Access & Passwords](#user-access--passwords)
4. [Network Security & Remote Access](#network-security--remote-access)
5. [Apache Webserver (HTTPD)](#apache-webserver-httpd)

---

## System Installation & Patching

1. **Protect the machine from hostile network traffic**  
   If the machine is a new install, protect it from hostile network traffic until the operating system is installed and hardened.

2. **Use the latest version of the Operating System**  
   Use the latest version of the OS and refer to the vendor’s support documentation to confirm the lifecycle of the version.

3. **Create a separate volume for `/tmp` with security options**  
   Create a separate volume with `nodev`, `nosuid`, and `noexec` options set for `/tmp` to prevent misuse:
   - `nodev`: Prevents users from creating or using block or special character devices.
   - `nosuid`: Prevents users from creating set-user-ID files in `/tmp`.
   - `noexec`: Prevents users from executing binaries from `/tmp`.

4. **Create separate volumes for `/var`, `/var/log`, and `/home`**  
   Separate these volumes from the root to limit the impact of non-admin users writing to them.

5. **Set the sticky bit on all world-writable directories**  
   The sticky bit ensures users cannot delete files owned by others in world-writable directories.

6. **Ensure the system is configured to receive software updates**  
   For RHEL or SLES, this requires an allocated subscription. For other distributions, it's a simple configuration change.

---

## OS Hardening

1. **Restrict core dumps**  
   Disable or restrict core dumps as they may contain sensitive or confidential data from memory.

2. **Remove legacy services**  
   Disable services that provide unencrypted authentication (e.g., telnet, rsh, tftp, talk).

3. **Disable unused services started by `xinetd` or `inetd`**  
   Remove `xinetd` if possible, and disable all unused applications managed by it.

4. **Disable or remove unneeded server services**  
   Services like FTP, DNS, LDAP, SMB, DHCP, NFS, and SNMP should be removed if not required.

5. **Ensure the syslog service is running**  
   Syslog services like `rsyslog`, `syslog-ng` manage logs in `/var/log/`. They should be running to track system activity.

6. **Enable a Network Time Protocol (NTP) service**  
   Accurate system time is critical for log analysis and auditing.

7. **Restrict the use of `cron` and `at` services**  
   Only allow accounts that require this access to use `cron` and `at`.

---

## User Access & Passwords

1. **Create a unique account for each user**  
   Avoid shared accounts to maintain an audit trail and easily revoke access when necessary.

2. **Enforce strong password policies**  
   Password security can be enforced via `/etc/pam.d/password-auth`.

3. **Use `sudo` for delegating admin access**  
   Use `sudo` to grant users specific admin privileges. Edit the `/etc/sudoers` file using `visudo`.

---

## Network Security & Remote Access

1. **Limit connections to authorized users only**  
   Use firewalls and access control technologies (e.g., `iptables`) to limit connections to services.

2. **Disable or configure kernel parameters**  
   - Disable IP forwarding, send packet redirects, and source routed packets.
   - Enable security options like ignoring broadcast requests and TCP/SYN cookies.
   - Configure these in `/etc/sysctl.conf`.

3. **Secure SSH configuration**  
   Ensure the following in `/etc/ssh/sshd_config`:
   - `Protocol 2`
   - `LogLevel INFO`
   - `PermitEmptyPasswords No`

4. **Disable root login over SSH**  
   Disallow root login over SSH to enhance security. Set `PermitRootLogin` to `no`, `without-password`, or `forced-commands-only`.

5. **Deploy an Intrusion Prevention System (IPS)**  
   Use tools like `fail2ban` to prevent brute-force attacks by blocking IPs with too many failed login attempts.

---

## Apache Webserver (HTTPD)

1. **Run Apache with a dedicated non-admin account**  
   Apache should run with a user account that has minimal system permissions.

2. **Disable unnecessary Apache modules**  
   Disable modules such as `webdav`, `status`, `info`, `userdir`, and `autoindex` unless required for your use case.

3. **Disable HTTP Trace**  
   Set `TraceEnable Off` to disable the HTTP TRACE method, which can be exploited in certain attacks.

4. **Configure SSL according to best practices**  
   Follow Mozilla’s [Server Side TLS](https://wiki.mozilla.org/Security/Server_Side_TLS) guidelines for secure SSL configuration.

5. **Hide software/OS versions in Apache headers**  
   Set `ServerTokens Prod` and `ServerSignature Off` to minimize exposure of system information.

6. **Restrict directory access**  
   By default, deny access to all files except those located in designated directories containing Apache content.

---

## Conclusion

Hardening a Linux system involves securing the operating system installation, configuring user access controls, enhancing network security, and ensuring specific services such as Apache are secure. Following this checklist will help ensure that the system is configured to minimize the attack surface and protect sensitive data.

---

## License

This checklist is licensed under the [MIT License](https://opensource.org/licenses/MIT). See the [LICENSE](LICENSE) file for more information.
```

---

This `README.md` file provides a clear, structured guide for hardening a Linux system. It includes sections on installation, OS hardening, user management, network security, and service-specific hardening (e.g., Apache), as well as instructions to improve overall system security.
