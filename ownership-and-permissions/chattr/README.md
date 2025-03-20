# Guide to `chattr` Command - File/Directory Attributes

The `chattr` command allows you to change file or directory attributes in Linux. It is maintained by the kernel and provides ways to modify the behavior of files or directories, offering features beyond traditional permissions.

## 1. Immutable Attribute (Prevent Deletion by Root)

To prevent a file or directory from being deleted, even by the root user, you can make it immutable.

### Command to make a file or directory immutable:

```bash
chattr +i <file-name>
chattr +i hh
```

- `+i` means **immutable**, making the file or directory unchangeable. If the file or directory doesn't have write (`w`) permission, it cannot be deleted.

### Command to remove the immutable attribute:

```bash
chattr -i <file-name>
```

### Command to check file or directory attributes:

```bash
lsattr hh
```

**Example Output:**

```bash
--i--------e- hh
```

- `i` indicates the **immutable** attribute.
- `e` indicates an attribute applied by the kernel.

## 2. Append-Only Attribute (Allow Only Appending Data)

To allow data to be appended to a file or directory without allowing deletion or overwriting, you can use the append-only attribute.

### Command to make a file or directory append-only:

```bash
chattr +a <file-name>
chattr +a hh
```

- `+a` means **append-only**: The file can only have data added and cannot be overwritten or deleted.

### Command to remove the append-only attribute:

```bash
chattr -a hh
```

---

## License

This document is licensed under the MIT License. See the LICENSE file for more details.
