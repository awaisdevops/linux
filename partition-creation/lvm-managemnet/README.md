# LVM Management Guide

This document provides a comprehensive guide on managing Logical Volume Management (LVM) on Linux systems, focusing on creating, extending, reducing, and removing logical volumes, as well as understanding the LVM structure and configuration.

## Table of Contents
1. [LVM Theory](#lvm-theory)
2. [Adding a New Disk and Creating LVM Partition (CentOS6)](#adding-a-new-disk-and-creating-lvm-partition-centos6)
3. [Adding a New Disk and Creating LVM Partition (CentOS7/8)](#adding-a-new-disk-and-creating-lvm-partition-centos78)
4. [Increasing LVM by Extending VG](#increasing-lvm-by-extending-vg)
5. [Increasing LVM by Extending PV/Adding a New Partition](#increasing-lvm-by-extending-pv-adding-a-new-partition)
6. [Increasing LVM when No Space Left on HDD](#increasing-lvm-when-no-space-left-on-hdd)
7. [Reducing LVM](#reducing-lvm)
8. [Removing LVM](#removing-lvm)
9. [How to Find LVM Layer Mappings](#how-to-find-lvm-layer-mappings)
10. [LVM Structure Backup and Restore](#lvm-structure-backup-and-restore)
11. [How to Take Snapshot of Logical Volume and Restore](#how-to-take-snapshot-of-logical-volume-and-restore)
12. [LVM Important Configuration Changes](#lvm-important-configuration-changes)
13. [Physical Extent (PE)](#physical-extent-pe)
14. [License](#license)

---

## LVM Theory
LVM is a logical device layer above the regular partitions that allows resizing partitions (either increasing or decreasing their size). LVM is used to create flexible and scalable disk structures, and it can handle partition resizing without the need to reformat partitions.

To check if your system has LVM, use:
```
lvmdiskscan
df -h
```

### LVM Components
- **Physical Volume (PV)**: The actual disk or partition where data is stored.
- **Volume Group (VG)**: A collection of physical volumes that provide storage for logical volumes.
- **Logical Volume (LV)**: The partition or volume created within a volume group that is used to store data.

---

## Adding a New Disk and Creating LVM Partition (CentOS6)
### Scenario: Create an LVM partition of 300 MB on a new 500 MB disk.

1. Add a 500 MB disk and create a partition of 400 MB. Change the partition type to `8e` (Linux LVM partition).
2. Update the kernel table:
    ```
    partx -a /dev/sdc
    ```
3. Create a physical volume:
    ```
    pvcreate /dev/sdb1
    pvdisplay
    ```
4. Create a volume group:
    ```
    vgcreate myvol /dev/sdb1
    vgdisplay
    ```
5. Create a logical volume of 300 MB:
    ```
    lvcreate -L 300M -n lv1 myvol
    lvdisplay
    ```
6. Format the logical volume:
    ```
    mkfs.ext4 /dev/myvol/lv1
    ```
7. Create a mount point and mount the volume:
    ```
    mkdir /common
    mount /dev/myvol/lv1 /common
    df -h
    ```
8. Add an entry in `/etc/fstab`:
    ```
    /dev/myvol/lv1  /common ext4    defaults        1 2
    mount -a
    ```

---

## Adding a New Disk and Creating LVM Partition (CentOS7/8)
The steps are the same as CentOS6, except for the following:
1. Use `xfs_growfs` instead of `resize2fs` for resizing.
2. LVM formatted with XFS cannot be reduced (this is a limitation of XFS, not `lvreduce`).

---

## Increasing LVM by Extending VG
If there is space available in the Volume Group (VG), you can extend the Logical Volume (LV).

1. Check the free space in the VG:
    ```
    vgdisplay | grep -i size
    ```
    Example output:
    ```
    Free  PE / Size       179 / 716.00 MiB
    ```
2. Extend the logical volume:
    ```
    lvextend -L +100M /dev/myvol/lv1
    ```
    Or, extend the LV to the total size:
    ```
    lvextend -L 400M /dev/myvol/lv1
    ```
3. Resize the file system:
    ```
    resize2fs /dev/myvol/lv1
    df -h
    ```

---

## Increasing LVM by Extending PV/Adding a New Partition
If there is no space left in the VG, add a new partition, create a new PV on it, and extend the VG.

1. Create a new partition and update the kernel table:
    ```
    partx -a /dev/sdb
    ```
2. Create a new PV:
    ```
    pvcreate /dev/sdb2
    pvdisplay
    ```
3. Extend the existing VG:
    ```
    vgextend pracvg /dev/sdb2
    vgdisplay
    ```
4. Extend the existing LV:
    ```
    lvextend -l +50%free /dev/pracvg/lv2
    resize2fs /dev/pracvg/lv2
    df -h
    ```

---

## Increasing LVM When No Space Left on HDD
1. Add a new HDD and create a new partition or create a PV on the entire HDD:
    ```
    pvcreate /dev/sdc
    ```
2. Extend the VG:
    ```
    vgextend pracvg /dev/sdc
    vgdisplay
    ```
3. Extend the LV or create a new LV as required.

---

## Reducing LVM
LVM reduction requires downtime as the mount point must be unmounted.

1. Unmount the LVM:
    ```
    umount /common/
    ```
2. Check the filesystem for repair:
    ```
    e2fsck -f /dev/myvol/lv1
    ```
3. Resize the filesystem:
    ```
    resize2fs /dev/myvol/lv1 200M
    ```
4. Reduce the LVM size:
    ```
    lvreduce -L 200M /dev/myvol/lv1
    mount /dev/myvol/lv1 /common/
    df -h
    ```

---

## Removing LVM
To remove LVM, remove the layers in reverse order: LV, VG, then PV.

1. Unmount the LVM:
    ```
    umount /common/
    ```
2. Remove the LV:
    ```
    lvremove /dev/myvol/lv1
    vgremove myvol
    pvremove /dev/sdb1
    ```

---

## How to Find LVM Layer Mappings
Use the following commands to view mappings of PV, VG, and LV:

1. `df -h`
2. `lsblk`
3. `pvs`
4. `vgs`
5. `lvs`
6. `pvscan`
7. `vgscan`
8. `lvscan`

---

## LVM Structure Backup and Restore
1. Backup the LVM structure:
    ```
    vgcfgbackup pracvg
    ```
2. Restore the LVM structure:
    ```
    vgcfgrestore myvol
    ```

---

## How to Take Snapshot of Logical Volume and Restore
To take a snapshot of an LV and restore it, refer to this [Tecmint guide](https://www.tecmint.com/take-snapshot-of-logical-volume-and-restore-in-lvm/).

---

## LVM Important Configuration Changes
1. To configure LVM, edit the `/etc/lvm/lvm.conf` file.
2. Set backup directory:
    ```
    /backup_dir = "/etc/lvm/backup"
    ```
3. To restrict which HDDs can be used for LVM, configure the `/filters` option in the LVM config file.

---

## Physical Extent (PE)
- **Physical Extents (PE)**: Data is stored in blocks called Physical Extents. The default size is 4 MB, but it can be adjusted.
- The **Logical Extent (LE)** is mapped linearly with PE.
- Use the following commands to manage PEs:
    ```
    vgdisplay | grep -i PE
    lvcreate -l 15 -n lv5 pracvg
    ```

---

## License
This guide is licensed under the MIT License.
