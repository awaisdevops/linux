# Linux Runlevels and Targets Guide

This guide explains the concept of **runlevels** in CentOS 6 and **targets** in CentOS 7/8. It provides instructions for switching runlevels, managing virtual consoles, recovering passwords, and securing single-user mode.

## Runlevels (CentOS 6) / Targets (CentOS 7/8)

Runlevels (CentOS 6) or Targets (CentOS 7/8) represent different OS working behaviors. There are a total of **7 runlevels** (0-6) in CentOS 6, which are essentially scripts that, when called, change the OS's working state.

### Checking and Changing Runlevels

To check the current runlevel:

```bash
cat /etc/inittab
o/p runlevel 5
```

- Output example: `N 5` (Current runlevel set to 5, with the previous being N)
- To change to a specific runlevel:

```bash
init <required-runlevel>
```

### Runlevels and Virtual Consoles

Runlevels 2, 3, and 4 offer **6 virtual consoles (VCS)**, which allow simultaneous logins by up to 6 users.

#### Switching Virtual Consoles

To switch between virtual consoles, use the following keyboard shortcuts:

- `Alt + F1`, `Alt + F2`, `Alt + F3`, `Alt + F4`, `Alt + F5`, `Alt + F6`

To check which virtual console you're on, use the `ps` command. The virtual console will be identified by its driver (e.g., `/dev/tty1`, `/dev/tty2`).

### Runlevel 5 and Gnome-Terminals

Runlevel 5 offers about **400 GUI terminals** (Gnome-terminals), depending on the Linux distro.

- To open a new Gnome-terminal: `Ctrl + N`
- To quit the terminal: `Ctrl + Q`

To check which terminal you're on, use the `ps` command. The terminal will be identified by its driver (e.g., `/dev/pts/1`, `/dev/pts/2`).

#### Temporarily Loading Graphics

In runlevel 3, you can temporarily load graphics using:

```bash
startx
```

To kill the graphics process and return to the virtual console:

```bash
pkill x
```

### Boot Process and Runlevel Configuration

During the boot process, the system reads the `/etc/inittab` file to load the set runlevel. You can change the default runlevel by modifying the `id` value:

```bash
id:5:initdefault
```

## How to Recover Password in CentOS 6

To reset the password, boot the machine using **runlevel 1** (single-user mode):

1. Interrupt the splash screen during boot by using the up and down arrow keys.
2. Press `a` to modify kernel arguments.
3. Remove `selinux=1` and add `1` in front of `rhgb quiet`.
4. The system will boot in safe mode. Change your password at the terminal and reboot.

## Securing Single-User Mode / Runlevel 1 in CentOS 6

To secure **runlevel 1** (single-user mode), edit the `/etc/sysconfig/init` file:

```bash
vim /etc/sysconfig/init
```

Change the line from:

```bash
SINGLE=/sbin/sushell
```

To:

```bash
SINGLE=/sbin/sulogin
```

This will prompt for a password in single-user mode.

### If Root Password is Forgotten

If you forgot the root password and secured single-user mode, you must login in **rescue mode** using a DVD or ISO file.

## Recovering Root Password in CentOS 7/8

In CentOS 7/8, there's no single-user mode. Instead, break the temporary filesystem (initramfs) to recover the password:

1. Shutdown the system: 

```bash
shutdown -h now
```

2. Interrupt the boot process by using the up and down arrows to access the splash screen.
3. Press `e` to edit the boot parameters.
4. In the line starting with `linux16 /vmlinuz...`, add `rd.break` at the end.
   
   This breaks the boot process at initramfs.

5. Press `Ctrl + X` to continue.

6. Remount the read-only filesystem in write mode:

```bash
mount -o remount,rw /sysroot
```

7. Change the shell to the filesystem’s root:

```bash
chroot /sysroot
```

8. Reset the root password:

```bash
passwd root
```

9. Re-label the file system:

```bash
touch /.autorelabel
```

10. Exit the shell twice:

```bash
exit
```

## Targets in CentOS 7/8

In CentOS 7/8, runlevels are replaced by **targets**. The two main targets are:

1. `graphical.target` - Equivalent to runlevel 5 in CentOS 6 (with GUI).
2. `multi-user.target` - Equivalent to runlevel 3 in CentOS 6 (without GUI).

### Switching Between Targets

To switch between targets:

```bash
systemctl isolate multi-user.target   # Switch to multi-user.target (runlevel 3)
systemctl isolate graphical.target   # Switch to graphical.target (runlevel 5)
```

To set the default target:

```bash
systemctl set-default graphical.target   # Set graphical.target as default
```

To print the current default target:

```bash
systemctl get-default
```

---
This guide provides a clear and structured format for understanding Linux runlevels and targets, with instructions for key actions, such as switching runlevels/targets and recovering passwords.
