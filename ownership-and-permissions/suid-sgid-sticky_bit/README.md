# Guide to SUID, SGID, and Sticky Bit in Linux

In Linux, SUID (Set User ID), SGID (Set Group ID), and Sticky Bit are advanced file permissions that allow specific behaviors to be applied to files and directories. These special permissions are useful when you need to give certain users or groups the ability to run root-privileged commands.

## Why Use SUID, SGID, and Sticky Bit?

SUID, SGID, and Sticky Bit are used when we want a particular user, group, or others to run root-privileged commands like `fdisk`, `mount`, `passwd`, and `shed`. 

**Important**: Setting SUID on root-privileged commands can allow all standard Linux users to run these commands with root privileges. This is a major security risk, but in cases where we need to grant root-privileged access to specific users, this is where these permissions come in. Alternatively, advanced permissions via the `sudoers` file can be used to assign more granular control.

### Example

You can set SUID on a command like `/usr/sbin/fdisk` and then switch users to execute `fdisk -l`. This will allow a normal user to execute the command.

### Why can every Linux user change their own password?

This is because SUID is set on the `passwd` command. To check this:

```bash
ls -lthr /usr/bin/passwd
```

**Example Output**:
```bash
-rwsr-xr-x 1 root root 59K Feb 6 17:54 /usr/bin/passwd
```

Here, the `s` in `rwsr-xr-x` indicates that the SUID bit is set on the `passwd` command.

---

## What Do SUID, SGID, and Sticky Bit Mean?

- **SUID (Set User ID)**: When applied to a file, it allows the file to run with the permissions of the file owner (usually root), not the user executing the file.
- **SGID (Set Group ID)**: When applied to a file, it allows the file to run with the permissions of the file's group.
- **Sticky Bit**: When applied to a directory, it ensures that only the file owner can delete or rename their files within the directory.

### Lowercase and Uppercase Representation

- **Lowercase `s` and `t`**: These indicate that the respective permission is applied with execute (`x`) permission.
- **Uppercase `S` and `T`**: These indicate that the respective permission is applied but without execute permission.

---

## Checking Files with SUID, SGID, and Sticky Bit

To find files that have these special permissions applied, use the following `find` commands:

- **Find files with SUID**:
  ```bash
  find / -perm -4000 -print
  ```
- **Find files with SGID**:
  ```bash
  find / -perm -2000 -print
  ```
- **Find files with Sticky Bit**:
  ```bash
  find / -perm -1000 -print
  ```
- **Find files with SUID, SGID, and Sticky Bit**:
  ```bash
  find / -perm -7000 -print
  ```

---

## Setting SUID, SGID, and Sticky Bit Using Symbolic Mode

### 1. SUID

To set the SUID bit on a file:

```bash
chmod u+s /opt/file
```

To check:

```bash
ls -ltrh file
```

**Example Output**:
```bash
rwSr--r-- 1 root root 0 May 25 14:55 file
```

If the file already has the `x` permission, the `s` will be lowercase:

```bash
chmod u+x /opt/file
```

After applying execute permission:

```bash
ls -ltrh file
```

**Example Output**:
```bash
rwsr--r-- 1 root root 0 May 25 14:55 file
```

### 2. SGID

To set the SGID bit on a file:

```bash
chmod g+s /opt/file
```

To check:

```bash
ls -ltrh file
```

**Example Output**:
```bash
rwxr-Sr-- 1 root root 0 May 25 14:55 file
```

### 3. Sticky Bit

To set the Sticky Bit on a file:

```bash
chmod o+t /opt/file
```

To check:

```bash
ls -ltrh file
```

**Example Output**:
```bash
rwxr-xr-t 1 root root 0 May 25 14:55 file
```

If the `x` permission is removed from others:

```bash
chmod o-x /opt/file
```

**Example Output**:
```bash
rwxr-xr-T 1 root root 0 May 25 14:55 file
```

---

## Setting SUID, SGID, and Sticky Bit Using Octal Mode

When using octal mode, the first digit specifies the SUID, SGID, and Sticky Bit settings.

- **SUID (Octal 4)**: Adds 4 to the permission bits.
- **SGID (Octal 2)**: Adds 2 to the permission bits.
- **Sticky Bit (Octal 1)**: Adds 1 to the permission bits.

For example:

- **Set SUID using octal**:
  ```bash
  chmod 4644 /opt/file
  ```

- **Set SGID using octal**:
  ```bash
  chmod 2644 /opt/file
  ```

- **Set Sticky Bit using octal**:
  ```bash
  chmod 1644 /opt/file
  ```

---

## License

This document is licensed under the MIT License. See the LICENSE file for more details.
```

This `README.md` provides a comprehensive guide on using SUID, SGID, and Sticky Bit in Linux, detailing their functionality, usage, and examples both in symbolic and octal modes.
