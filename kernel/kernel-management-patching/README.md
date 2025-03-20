# Kernel Management Patching Guide

This guide explains how kernel management and patching works in Linux systems. It covers multiple kernel versions, updating the kernel, and the boot process.

## Overview

Linux systems can have multiple kernel versions installed, but only one kernel is active at any given time. When updating packages, newer versions overwrite the older ones, but kernel updates work differently. When a new kernel version is installed, the existing kernel is not overwritten. Both the old and new kernel versions are kept along with their respective `vmlinuz`, `config`, and `initrd` files.

### Kernel Files

- **vmlinuz**: The compressed kernel file that is loaded during boot.
- **config**: The kernel configuration file, which contains various kernel settings.
- **initrd**: The initial RAM disk image that is used to mount the root partition during the boot process.

For example:
```bash
vmlinuz-5.19.0-32-generic
config-5.19.0-32-generic
initrd.img-5.19.0-32-generic
```

When you install a new kernel version, you will get the following three files:
1. `System.map-6.5.0-35-generic` (kernel map file)
2. `config-6.5.0-35-generic` (kernel configuration file)
3. `initrd.img-6.5.0-35-generic` (root partition mount)

---

## Checking Kernel Information

To check information about your current kernel, you can inspect the kernel configuration file.

```bash
vim config-5.19.0-32-generic
```

- You can search for specific settings within the configuration file by using `:set ic` to make searches case-insensitive, followed by `/` and the term you are looking for.
  
  For example:
  
  ```bash
  /ntfs
  ```

- If you find that `ntfs` is not enabled (e.g., `ntfs is not set`), it means your current kernel does not support NTFS. You can enable it by installing the NTFS driver or enabling the NTFS kernel module. If you see `m` or `y`, the module is enabled.

### Checking the Current Kernel Version

To check the currently active kernel version, use the following command:

```bash
uname -r
# Example output: 6.5.0-35-generic
```

---

## Switching Between Kernels

To switch to a different kernel, you need to reboot the system and select the desired kernel version during the boot process. After switching the kernel, only the kernel drivers are updated. The user-level files remain unchanged. This process is called **updating the kernel space**.

---

## Conclusion

Kernel management patching ensures that new kernel versions are installed alongside the old versions, without overwriting the existing kernel. This allows you to switch between different kernel versions as needed. Managing and configuring the kernel involves inspecting the kernel configuration files and updating the kernel drivers.

---

---

## License

This guide is licensed under the [MIT License](https://opensource.org/licenses/MIT). See the [LICENSE](LICENSE) file for more information.
