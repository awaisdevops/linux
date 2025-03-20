# User Management

## Add/Create Users

To create a user:

```bash
useradd <user-name>
```

The user's information is stored in the `/etc/passwd` file.

## Add/Create Groups

To create a group:

```bash
groupadd <group-name>
```

To add a user to a group:

```bash
gpasswd -a <user-name> <group-name>
```

To add multiple users to a group:

```bash
gpasswd -M user-name1,user-name2,user-name3 <group-name>
```

To remove a user from a group:

```bash
deluser <user-name> <group-name>
```

## Changing Ownership and Group

To change the owner of a file or directory:

```bash
chown -R <owner-name> <file-or-directory>
```

For example:

```bash
chown johncena facebook.html
```

To change the group of a file or directory:

```bash
chgrp -R <group-name> <file-or-directory>
```

For example:

```bash
chgrp -R root facebook.html
```

## Changing Permissions

To change file permissions:

```bash
chmod -R <permissions> <file-or-directory>
```

For example, using the octal method:

```bash
chmod -R 775 facebook.html
```

Using symbolic method:

```bash
chmod -R ugo+rwx facebook.html
```

## Example Commands

- Add a new user:

```bash
useradd awais
```

- Check user info:

```bash
id awais
```

- Confirm user in `/etc/passwd`:

```bash
cat /etc/passwd
```

- Check who is logged in:

```bash
whoami
```

- Delete a user:

```bash
userdel <user-name>
```

Note: Deleting a user with the `userdel` command only removes the user entry from `/etc/passwd` and other related information, but it doesn't remove the user's home directory.

- Delete a user along with their home directory:

```bash
userdel -r batista2
```

- Set a user's password:

```bash
passwd <username-to-set-password>
```

For example:

```bash
passwd awais
```

- Create a new group:

```bash
groupadd <group-name>
```

- Verify group:

```bash
cat /etc/group
```

- Add a user to a group:

```bash
gpasswd -a awais lospollos
```

- Add multiple users to a group:

```bash
gpasswd -M awais,umair,abdul lospollos
```

## `useradd` Command

When we use the `useradd` command to create a user, it triggers about 70 processes in the background to manage the user creation.

Both `useradd` and `adduser` can be used to create users. Note that `adduser` is a symlink for `useradd`.

To find their locations:

```bash
/usr/sbin/useradd
```

```bash
/usr/sbin/adduser
```

## Modifying User Info

To modify user information while creating a user:

```bash
useradd -u 600 -d /opt/wwe/ -c "this is a comment" -s /bin/bash wwe
```

To modify a user's info after creation:

```bash
usermod -u 5000 -d /opt/johncena/ -c "this is test user" -s /bin/bash johncena
```

### Disabling Users

To disable a user:

- Comment the user's entry in `/etc/passwd`.
- Remove `x` in the second field of `/etc/passwd`, or replace it with `*`.
- Write `/sbin/nologin` in the last field of the user’s entry in `/etc/passwd`.
- Enter incorrect information in the last field of `/etc/shadow`.

## Locking a User

To lock a user:

```bash
passwd -l <username-to-lock>
```

For example:

```bash
passwd -l wwe
```

To unlock a user:

```bash
passwd -u <username-to-unlock>
```

For example:

```bash
passwd -u wwe
```

## How `useradd` Command Works

When you execute the `useradd` command, the kernel maintains a user's information database. This database contains details like the user’s home directory, password, user and group IDs (UID and GID), assigned shell, etc. Upon login, the kernel verifies the user based on this information.

The user information is stored in the following files:

1. `/etc/login.defs`
2. `/etc/default/useradd`

After the user is created, the kernel updates the following four user database files:

1. `/etc/passwd`
2. `/etc/shadow`
3. `/etc/group`
4. `/etc/gshadow`

When you create a user (e.g., `useradd ali`), the user information is written into these files. The kernel uses these files to validate the user’s credentials and assign appropriate permissions, groups, UID, etc.

Additionally, the system copies some user profile files from `/etc/skel/*` to the user’s home directory (`/home/<username>`).

If you want to add a custom file to be included in the user’s home directory, place it in `/etc/skel`.

## User Database Files

### 1. `/etc/passwd`

This file is used by the kernel to validate users. It has 7 fields:

```bash
root:x:0:0:root:/root:/bin/bash
```

- **Field 1:** Username
- **Field 2:** Password placeholder (`x` means password is stored in `/etc/shadow`)
- **Field 3:** UID
- **Field 4:** GID
- **Field 5:** GECOS field (user information like full name, phone number, etc.)
- **Field 6:** Home directory
- **Field 7:** Shell assigned to the user

### 2. `/etc/shadow`

This file contains password information and policies, including password expiration and status. It consists of 9 fields:

```bash
batista5:!!:19867:0:99999:7:::
```

- **Field 1:** Username
- **Field 2:** Encrypted password (`!!` means no password)
- **Field 3:** Last password change (in days)
- **Field 4:** Minimum days until password can be changed
- **Field 5:** Maximum days before password must be changed
- **Field 6:** Warning period before password expires
- **Field 7:** Inactive days before account is disabled
- **Field 8:** Expiration date (account expiry)
- **Field 9:** Reserved for future use

### 3. `/etc/group`

This file contains group information. When a user is created, a group with the same name as the username is created in this file.

## License
MIT License

This `README.md` provides a comprehensive guide to user management, including user and group creation, permissions, user modifications, and more.
