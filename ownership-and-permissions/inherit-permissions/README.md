# Guide to Inherited Permissions in Linux

This guide explains how to handle user and group management, ownership, and permissions in Linux, with an emphasis on inheritance and the commands used to manage these permissions.

## 1. Add/Create Users

To create a new user, use the `useradd` command:

```bash
useradd <user-name>
```

User details are stored in the `/etc/passwd` file.

## 2. Add/Create Groups

To create a new group, use the `groupadd` command:

```bash
groupadd <group-name>
```

### Adding a Single User to a Group

To add a user to an existing group:

```bash
gpasswd -a <user-name> <group-name>
```

### Adding Multiple Users to a Group

To add multiple users to a group at once:

```bash
gpasswd -M <user-name1>,<user-name2>,<user-name3> <group-name>
```

## 3. Changing Ownership

To change the owner of a file or directory, use the `chown` command:

```bash
chown -R <owner-name> <file-or-directory-name>
```

**Example:**

```bash
chown test facebook.html
```

## 4. Changing Group Ownership

To change the group associated with a file or directory, use the `chgrp` command:

```bash
chgrp -R <group-name> <file-or-directory-name>
```

**Example:**

```bash
chgrp -R root facebook.html
```

## 5. Changing Permissions (Access Mode)

To change the permissions of a file or directory, use the `chmod` command.

### Changing Permissions Using Octal Mode

You can set permissions using octal notation (e.g., `775` for read, write, and execute for the owner and group, and read and execute for others):

```bash
chmod -R 775 <file-or-directory-name>
```

### Changing Permissions Using Symbolic Mode

You can also set permissions using symbolic notation (e.g., `ugo+rwx` to grant read, write, and execute permissions to user, group, and others):

```bash
chmod -R ugo+rwx <file-or-directory-name>
```

## License

This document is licensed under the MIT License. See the LICENSE file for more details.
