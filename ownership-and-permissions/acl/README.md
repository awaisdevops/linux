# Guide to ACL (Access Control List) in Linux

ACLs (Access Control Lists) are used in Linux when traditional symbolic or octal permissions cannot handle complex file and directory permissions. ACLs provide more granular control over who can access a file or directory.

## What is ACL?

ACL allows you to assign permissions not only to the file owner but also to other users and groups, providing more flexibility than the standard user/group/others permission system.

## Setting ACL Permissions

The `setfacl` command is used to set ACLs for users, groups, and others on files or directories.

### Set ACL for a User

To set specific permissions for a user on a directory:

```bash
setfacl -Rm u:johncena:rwx test/
```

This command grants the user `johncena` read, write, and execute permissions (`rwx`) on the directory `test/`.

### Set ACL for a Group

To set specific permissions for a group on a directory:

```bash
setfacl -Rm g:batista:rwx test/
```

This command grants the group `batista` read, write, and execute permissions (`rwx`) on the directory `test/`.

### Set ACL for Others

To set specific permissions for others (i.e., users not in the owner or group) on a directory:

```bash
setfacl -Rm o:rwx test/
```

This command grants others read, write, and execute permissions (`rwx`) on the directory `test/`.

## Checking ACL on a File/Directory

To check the current ACLs applied to a file or directory, use the `getfacl` command:

```bash
getfacl test/
```

This will display the ACL permissions for the `test/` directory.

## Removing ACL Permissions

To remove ACL permissions from a user or group, use the `-Rx` flag with the `setfacl` command:

### Remove ACL for a User

```bash
setfacl -Rx u:johncena test/
```

This command removes the ACL for the user `johncena` from the `test/` directory.

### Remove ACL for a Group

```bash
setfacl -Rx g:batista test/
```

This command removes the ACL for the group `batista` from the `test/` directory.

---

## License

This document is licensed under the MIT License. See the LICENSE file for more details.
