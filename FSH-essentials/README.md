# Linux OS Installation Partition Standards

This guide outlines the recommended partition scheme for Linux installation, including the standard partitions and their respective purposes. Additionally, it covers the essential directories in a Linux system and explains why certain partitions should not be created.

## 1. Recommended Partition Scheme (7 Partitions)

While 7 partitions are recommended, it is not strictly necessary to use all of them. The minimum required partition for a Linux installation is just the root partition (`/`). If you install Linux in a single partition, directories like `/boot` and `/var` will be created as directories inside that partition.

### Partition Scheme for 15 GB Hard Disk

```text
/boot      512MB
/          2GB
swap       (Double the RAM size)
/var       4GB
/tmp       1GB
/usr       4GB
/home      1GB
```

### Purpose of Directories

| Location     | Purpose                                                                 |
|--------------|-------------------------------------------------------------------------|
| `/usr`       | Standard software installations, shared libraries.                      |
| `/usr/bin`   | User commands.                                                          |
| `/usr/sbin`  | Root commands.                                                          |
| `/home`      | Home directories where users store their personal data & config files. |
| `/boot`      | Files needed to start the boot process.                                 |
| `/dev`       | Device driver files, contains special device files for hardware access.|
| `/etc`       | Configuration files specific to the system.                             |
| `lost+found` | Contains recovered files after filesystem repairs.                      |
| `/lib`       | Shared object files (`.so` files).                                      |
| `/var`       | Files that dynamically change, such as databases and log files.         |
| `/opt`       | Optional directory for installing third-party software.                 |
| `/mnt`       | Used for temporary mount points.                                        |
| `/media`     | Used for mounting removable media (e.g., USB drives, CD/DVDs).          |

### Important Notes on Partitioning

- We **cannot** create separate partitions for `/dev`, `/lib`, `/bin`, `/sbin`, and `/etc` because these directories are essential for the functioning of the root filesystem (`/`). If these directories are mounted as separate partitions, unmounting them would make critical data inaccessible, leading to system instability.

- **Critical Directories**:
  - `/var/` — Stores logs.
  - `/usr/` — Contains all standard packages.
  - `/opt/` — Contains third-party packages.
  - `/lib/` — Contains shared object files (`.so` files).
  - `/etc/` — Contains configuration files.

## 2. Important Files and Directories

### /boot Directory

- `vmlinuz` is the most important file in `/boot/` — it is the kernel.
- `grub` is the boot loader directory.
  
### /var Directory

- `/var/` contains files that change dynamically, including logs and databases.

### /dev Directory

- `/dev/` contains device driver files. Here are a few examples:
  - `/dev/sda` is the driver for hard disk partitions.
  - `/dev/lp0` is the driver for printers.
  - `/dev/nst0` is the driver for LTO (tape) devices.

### Special Device Files

- `/dev/null`: Any input or output redirected to `/dev/null` will be discarded (nulled).

```bash
df -h > /dev/null
ls / > /dev/null
```

### /dev/loop0

- `/dev/loop0` is the driver for ISO files. You can mount ISO images using `loop0`. There are 8 loop devices (`loop0` to `loop7`), which means you can mount up to 8 ISO images. The number of loop devices can be increased if needed.

Example to mount an ISO:

```bash
mount -o loop /dev/loop0 centos.iso /mnt/
```

### Finding Device Files

- To find character files in `/dev/`:

```bash
ls -l /dev/* | grep ^c | wc -l
```

- To find socket files in `/dev/`:

```bash
ls -l /dev/* | grep ^s | wc -l
```

## Conclusion

This partitioning scheme and understanding of critical directories are essential when installing Linux. Proper partitioning can improve system performance, security, and manageability. However, avoid creating separate partitions for directories like `/dev`, `/lib`, `/bin`, `/sbin`, and `/etc` to ensure the root filesystem remains functional.

---

## License

This guide is licensed under the **MIT License**. You can freely use, modify, and distribute this guide. See the [LICENSE](LICENSE) file for more details.
