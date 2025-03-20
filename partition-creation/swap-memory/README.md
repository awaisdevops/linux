# Swap Memory in Linux

Swap is a type of virtual memory that serves as an extension of RAM in Linux systems. It is stored on the hard disk as a partition using Virtual File System (VFS). The swap space temporarily stores passive processes when RAM is full to make space for incoming processes, while active processes always reside in RAM.

In Windows, swap is referred to as "paging," which is dynamically managed by the OS.

## Why Use Swap Space?

Creating swap during OS installation is not mandatory but recommended in certain situations:

- **Low RAM**: If the system has limited RAM resources.
- **Application Requirements**: Some applications, especially databases, may require swap space for caching, as indicated in their documentation.

Swap space allows applications and files to be processed by moving less active processes from RAM to the swap partition. This enables RAM to be freed up for new processes.

## Checking Current Swap Space

To check the swap space on your system, you can use the following commands:

```bash
free -m
top
swapon -s
fdisk -l
```

## How to Extend Swap Space

### 1. Use a New Disk

If you have a new disk available, you can create a swap partition on it.

```bash
# Add a new disk (e.g., /dev/sdb) from your storage
fdisk /dev/sdb
# Set partition type to swap (82) using fdisk
partprobe /dev/sdb
partx /dev/sdb
# Create the swap space
mkswap /dev/sdb1
# Add the swap partition entry to /etc/fstab
vim /etc/fstab
```

Add this line to `/etc/fstab`:

```
UUID=<UUID> swap swap defaults 0 0
```

Then, enable the swap:

```bash
swapoff -a
swapon -a
swapon -s
```

Finally, regenerate the initramfs to ensure swap is recognized during boot:

```bash
dracut -f -v
```

### 2. New Partition on OS Disk

If there is free space on the original disk, you can create a new partition and convert it into swap.

```bash
# Create a new partition on the original disk
fdisk /dev/sda
# Create swap space on the new partition
mkswap /dev/sda4
# Add the swap partition entry to /etc/fstab
vim /etc/fstab
```

Then, enable the swap:

```bash
swapon -a
swapon -s
dracut -f -v
```

### 3. Create a Swap File

If there is no free space available, you can create a swap file.

```bash
# Create a swap directory
mkdir /swap
# Create a swap file (e.g., 2GB)
dd if=/dev/zero of=/swap/swapfile1 bs=1M count=2048
# Create the swap space
mkswap /swap/swapfile1
# Change permissions
chmod 600 /swap/swapfile1
# Add entry to /etc/fstab
vim /etc/fstab
```

Add this line to `/etc/fstab`:

```
/swap/swapfile1 swap swap defaults 0 0
```

Mount and enable the swap:

```bash
mount -a
swapon -a
swapon -s
free -m
```

## Increasing Swap Space

To increase the swap space, simply turn off swap and increase the size of the swap file or partition.

```bash
swapoff /swap/swapfile1
dd if=/dev/zero of=/swap/swapfile1 bs=1M count=4096  # Increase swap file size
mkswap /swap/swapfile1
chmod 600 /swap/swapfile1
vim /etc/fstab
mount -a
swapon -a
swapon -s
free -m
```

## Deleting Swap Space

To delete swap created using a swap file:

```bash
swapoff -a -v
rm -rf /swapfile
cp /etc/fstab /etc/fstab.bak
vim /etc/fstab
```

Remove the swap entry from `/etc/fstab`.

## Identifying Processes Using Swap Space

To find out which processes are using swap, you can check the process information:

```bash
ps aux | grep -i gnome-volume-*
cd /proc/
cd <PID>
less status
cat status | grep -i swap
```

You can also use a script to list processes using swap:

### Script: `swapspace.sh`

```bash
#!/bin/bash
# Get current swap usage for all running processes
# Erik Ljungstrom 27/05/2011
# Modified by Mikko Rantalainen 2012-08-09
# Modified by Marc Methot 2014-09-18
# removed the need for sudo

SUM=0
OVERALL=0
for DIR in `find /proc/ -maxdepth 1 -type d -regex "^/proc/[0-9]+"`
do
    PID=`echo $DIR | cut -d / -f 3`
    PROGNAME=`ps -p $PID -o comm --no-headers`
    for SWAP in `grep VmSwap $DIR/status 2>/dev/null | awk '{ print $2 }'`
    do
        let SUM=$SUM+$SWAP
    done
    if (( $SUM > 0 )); then
        echo "PID=$PID swapped $SUM KB ($PROGNAME)"
    fi
    let OVERALL=$OVERALL+$SUM
    SUM=0
done
echo "Overall swap used: $OVERALL KB"
```

Save this script as `/opt/swapspace` and run it:

```bash
bash /opt/swapspace
```

## License

This document is licensed under the MIT License. See the LICENSE file for details.

This `README.md` file is formatted for GitHub and provides clear instructions for managing swap memory on a Linux system, from creating, extending, and deleting swap space to finding processes using swap.
