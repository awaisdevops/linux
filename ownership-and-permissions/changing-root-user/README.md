# Converting a Standard User to Root User

In Linux, the concept of a "root" user is not tied to a specific user account name. Any user with a UID (User ID) and GID (Group ID) of 0 is considered an administrator (or root user). This could be `root`, or any other user, such as `rondarosy` or `batista`, if their UID and GID are set to 0. The kernel uses the UID and GID to determine the administrator status.

## Check User ID (UID) and Group ID (GID)

To check the UID and GID of a user:

```bash
id <username>
```

For the root user:

```bash
id root
```

The output will be:

```bash
uid=0(root) gid=0(root) groups=0(root)
```

This shows that the root user's UID and GID are both 0, which grants it root privileges.

## Making a Standard User as a Root User

To convert a standard user to a root user, we can modify the `sudoers` file or directly change the user's UID to 0.

### Method 1: Modify the `sudoers` File

1. Open the `sudoers` file for editing using the `visudo` command:

   ```bash
   sudo visudo
   ```

2. Locate the following line in the `sudoers` file:

   ```
   # User privilege specification
   root    ALL=(ALL:ALL) ALL
   ```

3. Replace `root` with the desired username, such as `rondarosy`:

   ```
   rondarosy    ALL=(ALL:ALL) ALL
   ```

This will grant the user `rondarosy` the same privileges as the root user.

---

### Method 2: Change UID and GID to 0

You can also make a user an admin by changing their UID and GID to 0.

1. Use the `usermod` command to change the UID to 0:

   ```bash
   sudo usermod -u 0 rondarosy
   ```

2. Edit the `/etc/passwd` file to change the third and fourth fields (the UID and GID) for `rondarosy` from 1001:1002 (for example) to 0:0:

   ```bash
   sudo vim /etc/passwd
   ```

   Find the line for `rondarosy` and update it to:

   ```
   rondarosy:x:0:0:User:/home/rondarosy:/bin/bash
   ```

3. Check if the UID and GID have been successfully changed:

   ```bash
   id rondarosy
   ```

   The output will show:

   ```bash
   uid=0(root) gid=0(root) groups=0(root)
   ```

   Now, the user `rondarosy` has root privileges.

---

### Reverting Changes

**Important**: Do not try these changes on a production server unless you fully understand the implications. Always ensure you have proper backups before making such critical modifications.

To revert the changes, restore the previous UID and GID of the user, and remove any modifications made to the `sudoers` file.

---

## Conclusion

In Linux, the root user is simply a user with UID and GID set to 0. You can make any user a root user by either modifying the `sudoers` file or directly changing the user's UID and GID. However, modifying UID and GID should be done with caution and proper backups, especially on production systems.
