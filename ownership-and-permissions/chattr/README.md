# Guide to `chattr` Command and ACLs (Advanced Permissions)

This guide explains how to use the `chattr` command to manage file and directory attributes, as well as how to use Access Control Lists (ACLs) for advanced permission management in Linux.

## File/Directory Attributes: `chattr` Command

The `chattr` command allows us to change file and directory attributes. It is maintained by the Linux kernel and provides a way to modify file behaviors that go beyond traditional file permissions.

### 1. **Make a File/Directory Immutable (Prevent Deletion by Root)**

You can make a file or directory immutable, meaning it cannot be deleted or modified, even by the root user.

```bash
chattr +i <file-name>
chattr +i hh
```

- `+i` means **immutable**: The file cannot be modified or deleted.
- If the file/directory doesn't have write (`w`) permission, it cannot be deleted.
  
To remove the immutable attribute:

```bash
chattr -i <file-name>
```

To check the attributes of a file:

```bash
lsattr hh
```

**Example output:**

```bash
--i--------e- hh
```

- `i` indicates the **immutable** attribute.
- `e` indicates an attribute applied by the kernel.

### 2. **Append-Only Attribute**

If you want a file or directory to only allow data to be appended (without deletion or overwriting), use the append-only attribute.

```bash
chattr +a <file-name>
chattr +a hh
```

- `+a` means **append-only**: The file can only have data added, not overwritten or deleted.

To remove the append-only attribute:

```bash
chattr -a hh
```

## License

This document is licensed under the MIT License. See the LICENSE file for more details.
