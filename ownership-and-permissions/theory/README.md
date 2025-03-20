# Ownership and Permissions Guide

This guide explains the concepts of file ownership and permissions in Linux.

## File Permissions and Access Mode

Example of file details:
```
-rwxrwxr-x. 1 awaisakram root 10 Mar 29 11:53 faceook.html
```

These are the 9 fields of any file or directory in Linux:

1. `-` → Regular Linux file
2. `rwxrwxr-x` → 9 sub-fields representing permissions for **User**, **Group**, and **Others**.
3. `.` → SELinux is applied.
4. `1` → Hard link count (for a file) or the number of directories in a directory.
5. `awaisakram` → Owner of the file.
6. `root` → Group associated with the file.
7. `10` → Size of the file in bytes.
8. `Mar 29 11:53` → Last access time.
9. `faceook.html` → Name of the file or directory.

## Types of Files in Linux

There are 7 types of files in Linux:

1. `-` → Regular file (e.g., text file)
2. `l` → Symbolic link (symlink)
3. `d` → Directory
4. `b` → Block file
5. `c` → Character file
6. `s` → Socket file
7. `p` → Pipe device file

## File Permissions: `rwx`

### 1. **Read (r) Permission**
- **For files**: Allows reading the content of the file, and copying the file. To copy a file, it must have at least read permission.
- **For directories**: Allows listing the contents of the directory, including its sub-directories and files.

### 2. **Write (w) Permission**
- **For files**: Allows modifying the file, including editing or deleting it. Deleting a file requires write permission. To delete a file, it must also have read permission to avoid any complexities.
- **For directories**: Allows modifying the directory, such as creating subdirectories or files within it.

### 3. **Execute (x) Permission**
- **For files**: Allows executing the file, e.g., running scripts, binaries, etc.
- **For directories**: Allows entering the directory (i.e., using `cd` to change into the directory). A directory with execute permission means you can `cd` into it.
```
