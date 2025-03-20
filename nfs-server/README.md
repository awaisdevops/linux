# NFS (Network File System) Guide

NFS (Network File System) is a protocol used for sharing files over a network. It allows a server to share directories and files with clients, which can then mount and access the shared resources. This guide explains the process of configuring and using NFS for file sharing between Linux servers and clients.

## Overview

NFS is widely used to share data between Linux systems. It uses UDP port 2049 for communication. However, NFS does not encrypt data during transfer and does not require authentication, which can pose security risks. In contrast to protocols like FTP or Samba, NFS is specifically used for sharing data between Linux-based systems and does not provide the ability to download data directly.

## NFS Protocol/Service Configuration

This guide will walk you through the steps to configure NFS on both the server and client side, with examples for each.

### Server-Side NFS Configuration

1. **Check if NFS is installed:**

   ```bash
   rpm -qa | grep -i nfs-utils
   ```

2. **Install NFS package if not installed:**

   ```bash
   yum install nfs-utils -y
   ```

3. **Verify installation:**

   ```bash
   rpm -qa | grep -i nfs-utils
   ```

4. **Start and enable the NFS service:**

   ```bash
   systemctl status nfs
   systemctl restart nfs
   systemctl enable nfs
   ```

5. **Start and enable `rpcbind` (a dependency for NFS):**

   ```bash
   systemctl restart rpcbind
   systemctl enable rpcbind
   systemctl status rpcbind
   ```

6. **Verify that the NFS port (2049) is open:**

   ```bash
   netstat -tulnp | grep -i 2049
   ss -tulnp | grep -i 2049
   ```

7. **Allow NFS services in the firewall:**

   ```bash
   firewall-cmd --permanent --add-service mountd
   firewall-cmd --permanent --add-service rpc-bind
   firewall-cmd --permanent --add-service nfs
   firewall-cmd --reload
   ```

8. **Configure the shared directory in `/etc/exports`:**

   Edit the `/etc/exports` file to specify the directories to share and their permissions.

   ```bash
   vim /etc/exports
   ```

   Example entry:

   ```bash
   /opt/data *(sync)
   ```

   - `sync` ensures that the changes are written to the disk before returning control to the client.
   
9. **Reload the NFS export configuration:**

   ```bash
   exportfs -rv
   ```

10. **Verify the shared directories:**

    ```bash
    exportfs -v
    ```

    Example output:

    ```bash
    /etc/data <world>(sync,wdelay,hide,no_subtree_check,sec=sys,ro,secure,root_squash,no_all_squash)
    ```

    - To allow write access to the shared directory, modify the permissions in `/etc/exports`:

      ```bash
      vim /etc/exports
      ```

      Change the line to allow read-write access:

      ```bash
      /etc/data *(sync,rw)
      ```

    - Change the directory permissions:

      ```bash
      chmod -R 777 /etc/data/
      ```

    - Reload the NFS configuration:

      ```bash
      exportfs -rv
      ```

### Client-Side NFS Configuration

1. **Install NFS client utilities:**

   ```bash
   rpm -qa | grep -i nfs-utils
   yum install nfs-utils -y
   rpm -qa | grep -i nfs-utils
   ```

2. **Start and enable the NFS client service:**

   ```bash
   systemctl status nfs
   systemctl restart nfs
   systemctl enable nfs
   ```

3. **Start and enable `rpcbind` on the client machine:**

   ```bash
   systemctl restart rpcbind
   systemctl enable rpcbind
   systemctl status rpcbind
   ```

4. **Check the shared directories available from the NFS server:**

   ```bash
   showmount -e server1
   showmount -e 192.168.3.135
   ```

   This will show the list of shared directories on the NFS server.

5. **Create a directory on the client to mount the NFS share:**

   ```bash
   mkdir /myshare
   ```

6. **Mount the shared directory from the NFS server:**

   ```bash
   mount server1:/etc/data /myshare/
   ```

7. **Verify that the mount was successful:**

   ```bash
   df -h
   ```

   Example output:

   ```bash
   server1:/etc/data  22G  3.9G  19G  18% /myshare
   ```

8. **Make the mount persistent across reboots:**

   Edit the `/etc/fstab` file to add the NFS mount:

   ```bash
   vim /etc/fstab
   ```

   Add the following line:

   ```bash
   server1:/etc/data       /myshare        nfs     defaults        0 0
   ```

9. **Verify the integrity of the fstab file and mount all entries:**

   ```bash
   mount -a
   ```

### Testing the NFS Share

- To verify the setup, create files on the server in the shared directory and check if they appear on the client.
- Similarly, create files on the client and check if they appear on the server.

### Default NFS Permissions

In `/etc/exports`, you will see the default permission settings for NFS:

```bash
/etc/data <world>(sync,wdelay,hide,no_subtree_check,sec=sys,ro,secure,root_squash,no_all_squash)
```

- These are the default permissions for shared directories, and you can modify them based on your requirements.

## Conclusion

NFS provides a powerful way to share files between Linux systems over a network. This guide covered both the server-side and client-side configuration of NFS, including setting up shared directories, configuring permissions, and making the mounts persistent. Remember to always consider security and access control when setting up NFS in production environments.

---

## License

This guide is licensed under the [MIT License](https://opensource.org/licenses/MIT). See the [LICENSE](LICENSE) file for more information.
```

---

This `README.md` provides a step-by-step guide on how to configure and use NFS for sharing files between Linux systems, with server and client configuration instructions, as well as security considerations. The license section at the end can be modified based on your specific requirements.
