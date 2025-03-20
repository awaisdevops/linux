# Guide to Partition Creation in Linux

## Structure and Working of Hard Disk

A hard disk consists of a magnetic platter with tracks divided into sectors. The head, located just above the platter, writes and retrieves data from the platter using magnetism. As the platter rotates, the head writes data to different sectors, and data is calculated in cylindrical form.

When a new disk is added from SAS, NAS, SAN, etc., the kernel assigns it a device driver (e.g., `/dev/sdc`). Upon partitioning the disk, each partition gets a new driver (e.g., `/dev/sdc1`, `/dev/sdc2`, etc.), which allows the kernel to access these partitions.

### Tools for Partitioning:
- `fdisk`: Standard tool for partitioning disks, supports partitions up to 2 TB.
- `cfdisk`: A more user-friendly partitioning tool.
- `sfdisk`: A scriptable partition tool.
- `parted`: Used for creating, deleting, and resizing partitions.

The most common tool used is `fdisk`.

## Steps to Create Partitions

### 1. Add a New Disk

First, ensure the new disk is detected by the system:

```bash
echo "- - -" > /sys/class/scsi_host/host0/scan
echo "- - -" > /sys/class/scsi_host/host1/scan
echo "- - -" > /sys/class/scsi_host/host2/scan
```

Run these commands three times for each host (`host0`, `host1`, and `host2`). Then, confirm that the disk has been added:

```bash
cat /dev/sd*
```

The new disk should appear (e.g., `/dev/sdb`).

### 2. Create Partitions

Run `fdisk` to partition the disk:

```bash
fdisk /dev/sdb
```

Follow the prompts to create partitions as per your requirements. After you’ve finished creating partitions, press `w` to write the partition table and exit.

### 3. Update Kernel Table

Use `partx` to update the partition table:

```bash
partx -a /dev/sdb
```

Run the command three times to ensure the kernel table is updated correctly.

Alternatively, use `partprobe`:

```bash
partprobe -s
```

This is used on CentOS 7/8 to update the kernel table.

### 4. Format the Partition

Next, format the partition with a file system (e.g., `ext4`):

```bash
mkfs.ext4 /dev/sdb1
```

This command formats the partition and sets up the file system. Formatting a partition means creating the file system, which involves indexing the data. This indexing information is called metadata.

### 5. Mount the Partition

Create a directory to mount the partition:

```bash
mkdir /tomcat
```

Then, mount the partition:

```bash
mount /dev/sdb1 /tomcat/
```

### 6. Make Mount Permanent

To ensure the partition is mounted automatically at boot, add an entry to `/etc/fstab`:

```bash
vim /etc/fstab
```

Add the following line:

```
/dev/sdb1               /tomcat          ext4         defaults        0              0
```

Then, verify the changes by running:

```bash
mount -a
```

This will auto-mount all entries in `/etc/fstab`. If there are errors, they will be displayed.

### 7. Unmount the Partition

If you need to unmount the partition:

```bash
umount /tomcat/
```

### 8. Confirm the Partition is Added

To confirm that the partition was added successfully:

```bash
df -Th
fdisk -l
```

---

## Precautions When Deleting a Partition

When deleting a partition, avoid deleting partitions in between others. Deleting a partition will shift the partition drivers, which can cause the file system specifications to change. This can result in booting issues where `/etc/fstab` fails to correctly reference the mount points, leading to system errors on reboot.

---

## Permissions Field in `/etc/fstab`

The 4th field in `/etc/fstab` specifies the permissions of the directory or mount point. The following permissions are available:

- `ro`: Read-only.
- `rw`: Read-write.
- `exec`: Grants execute permission to the mount point.
- `noexec`: Denies execute permission.
- `auto`: Allows auto-mount during boot.
- `noauto`: Prevents auto-mount during boot; manual mounting is required.
- `user`: Allows users to mount the file system.
- `nouser`: Prevents users from mounting the file system (root-only).
  
To modify the permissions, replace `defaults` with the desired permission (e.g., `ro`, `noexec`, `auto`, etc.).

### Example

```
/dev/sdb1               /tomcat          ext4         rw,noexec        0              0
```

After changing the permissions, use the `umount` and `mount` commands to save the changes:

```bash
umount /tomcat
mount -o remount /tomcat
```

### Default Permissions

The default permissions include the following:

- `auto`
- `rw`
- `dev`
- `async`
- `suid`
- `exec`
- `nouser`

---

## UUID/Block ID

UUID (Universal Unique Identifier) is used to reference partitions instead of driver names in `/etc/fstab`. It’s recommended to use UUIDs because driver names may change. The kernel references UUIDs internally.

### Commands for UUID:

- To find the UUID of a partition:

  ```bash
  blkid /dev/sdb1
  ```

- List all UUIDs:

  ```bash
  ls -l /dev/disk/by-uuid/
  ```

- To view information about attached hard disks and partitions:

  ```bash
  lsblk -f
  ```

---

## Partition Table Types

There are two types of partition tables:

- **MSDOS/MBR**: Supports up to 4 primary partitions. You can also create one extended partition, which can have up to 63 logical partitions.
- **GPT**: Supports up to 128 primary partitions.

Use the following command to check the partition table type:

```bash
parted -l /dev/sdb
```

---

## File System Check and Repair (FSCK)

File systems can become corrupted due to unclean shutdowns, power fluctuations, or application errors. When a file system gets corrupted, it may contain bad sectors, inodes, or blocks.

### Manual FS Check and Repair:

1. Check for bad blocks:

   ```bash
   badblocks -v /dev/sdb1
   ```

2. Repair the file system using `e2fsck`:

   ```bash
   e2fsck /dev/sdb1
   ```

   Use the `-yc` flags to print a detailed repair report:

   ```bash
   e2fsck -yc /dev/sdb1
   ```

### Automatic FS Check

The 6th field in `/etc/fstab` determines the file system check priority. A value of `0` means no check will occur. A value of `1` is for the root partition (`/`), and `2` is for other partitions.

```bash
cat /etc/fstab
```

If `fsck` is enabled, assign `1` to `/` and `2` to other partitions for proper file system check and repair during boot.

### Can You Unmount `/` to Repair It?

No, you cannot unmount the root (`/`) partition to repair it while the system is running. You need to enter rescue mode via an ISO image/DVD to repair the root partition.

---

## License

This guide is released under the MIT License. See the [LICENSE](LICENSE) file for more details.

This `README.md` provides a detailed step-by-step guide for partition creation and management in Linux, covering topics like partition tools, mounting, permissions, UUID, partition types, and file system repair. It includes precautions, examples, and practical tips to avoid errors. 
