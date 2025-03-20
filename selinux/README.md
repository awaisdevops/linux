# SELinux Guide

## Overview

**Security-Enhanced Linux (SELinux)** is a Linux kernel security module that provides a mechanism for supporting access control security policies, including mandatory access controls (MAC). SELinux is a set of kernel modifications and user-space tools that have been integrated into various Linux distributions.

SELinux can be either enabled or disabled. Disabling SELinux turns off the kernel's security module, and the kernel will no longer monitor security access policies. Enabling SELinux comes with two options:

1. **Enforcing** – Security policies are strictly enforced.
2. **Permissive** – Violations are allowed but logged as warnings.

---

## SELinux Configuration

### Configuration File

To manage SELinux settings, edit the `/etc/selinux/config` file.

```bash
vim /etc/selinux/config
```

In the configuration file:
- **Enforcing** mode is assigned the value `1`. This means the kernel will strictly enforce security rules.
- **Permissive** mode is assigned the value `0`. In this mode, SELinux logs violations but does not enforce them.

### Enforcing Mode

When SELinux is in **Enforcing** mode, it provides two key options:

1. **Boolean Value** (On/Off, 1/0): Applied to services for additional security layers.
2. **Context Value**: Applied to files and directories for additional security layers.

---

## Setting SELinux Using Boolean Values on Services

When SELinux is enabled with the **Enforcing** mode, access to services like FTP or Samba may be controlled by SELinux boolean values.

For example:
- If a user is allowed access to the user's home directory for FTP in the configuration, but SELinux blocks it using the **boolean value** for FTP access, the user will not be able to access it. Access at the service level might be allowed, but SELinux will deny it at the kernel level.

### Checking SELinux Boolean Values for Services

To check all available SELinux boolean values:

```bash
getsebool -a
```

To filter results for specific services like **FTP** or **Samba**, use:

```bash
getsebool -a | grep -i ftp
getsebool -a | grep -i samba
```

### Example: Enabling Samba Home Directory Creation

If the **Samba** service has the boolean value `samba_create_home_dirs` set to `off`, the kernel will block the creation of home directories for Samba, even if Samba's configuration allows it.

To enable this boolean value:

```bash
setsebool -P samba_create_home_dirs on
```

To confirm the change:

```bash
getsebool -a | grep -i samba
```

This will show the new state of the boolean value, allowing the creation of Samba home directories.

---

## Setting SELinux Using Context Values on Files/Directories

SELinux contexts are applied to files and directories to control access.

### Example: FTP Directory Access

Consider the `/var/ftp` directory, which has a restrictive SELinux context. Even if the FTP configuration allows access, the user may still be denied access due to the SELinux context.

To check the SELinux context of a directory:

```bash
ls -Zd /var/ftp/
```

If the context allows access, the user can access the directory. Otherwise, access will be denied.

---

## Important Notes

- You can switch SELinux between **Enforcing** and **Permissive** modes without rebooting the system.
- Switching from **Enforcing** or **Permissive** mode to **Disabled** (or vice versa) **requires a system reboot**.

---

## Useful Commands

- **Check SELinux Status**:

  ```bash
  sestatus
  getenforce
  ```

- **SELinux Configuration File**:

  ```bash
  /etc/selinux/config
  ```

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

This `README.md` provides a structured guide for configuring SELinux on a Linux system, covering both basic concepts and practical commands.
