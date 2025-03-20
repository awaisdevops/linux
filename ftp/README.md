# FTP (File Transfer Protocol) Service Configuration Guide

FTP (File Transfer Protocol) is a service used for file transfer, allowing both upload and download of files. While NFS and Samba are also used for file sharing, FTP provides additional functionality to download data. FTP uses two ports:
- Port 21: Used for establishing the connection
- Port 20: Used for data transfer

---

## Types of FTP Servers

Here are some commonly used types of FTP servers:

1. **wuFTP** (Washington University FTP)
2. **ProFTPD** (Pro FTP Daemon)
3. **SFTP** (Secure FTP)
4. **VSFTPD** (Very Secure FTP Daemon)

Most financial institutions use **SFTP** due to its use of **SSH** to transfer data securely.

---

## Configuring FTP Server

### Server-Side Configurations

1. **Check if FTP server is installed**:

   ```bash
   rpm -qa | grep -i vsftpd-*
   ```

2. **Install VSFTPD package**:

   ```bash
   sudo yum install vsftpd-* -y
   ```

3. **Confirm installation**:

   ```bash
   rpm -qa | grep -i vsftpd-*
   ```

4. **Start and enable the VSFTPD service**:

   ```bash
   sudo systemctl start vsftpd
   sudo systemctl enable vsftpd
   sudo systemctl status vsftpd
   ```

5. **List all active services**:

   ```bash
   systemctl list-units
   systemctl list-unit-files
   systemctl list-units | grep -i vsftpd
   ```

   For CentOS 6, use:

   ```bash
   chkconfig --list
   ```

6. **Check FTP port and ensure it's listening on port 21**:

   ```bash
   netstat -tulnp | grep 21
   ss -tulnp | grep 21
   telnet 127.0.0.1 21
   nc 127.0.0.1 21
   ```

7. **Allow FTP ports on firewall**:

   ```bash
   ufw allow 20/tcp
   ufw allow 21/tcp
   ```

8. **Default path for FTP files**:

   ```bash
   cd /var/ftp
   ```

9. **List and modify the user list**:

   ```bash
   vim /etc/vsftpd/user_list
   ```

10. **Access FTP server** via browser:

    ```bash
    ftp://server1
    ftp://server_domain_name
    ftp://server_ip
    ```

11. **Create FTP users** and specify their home directories:

    ```bash
    useradd -d /ftpuser-us -s /sbin/nologin ftpuser-us
    ```

    Email the details to the user for access.

---

## Securing FTP Server

There are three types of FTP users:

1. **Anonymous FTP User**: A blank username and password are used for accessing public files (often used for downloading files).
2. **Local FTP User**: Users created on the FTP server with access defined in `/etc/passwd`.
3. **Virtual Users**: Users created specifically for FTP access, not mapped to system accounts.

### 1. Disabling Anonymous FTP User

1. **Backup the configuration file**:

   ```bash
   cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.bak
   ```

2. **Disable anonymous FTP access**:

   ```bash
   vim /etc/vsftpd/vsftpd.conf
   # Change: anonymous_enable=YES to anonymous_enable=NO
   ```

3. **Reload VSFTPD service**:

   ```bash
   systemctl reload vsftpd
   ```

4. **Verify FTP access**: Access the FTP server via a browser. It should now ask for credentials, confirming that anonymous access is disabled.

### 2. Jailing (Chroot) the User

To restrict users to their home directory and prevent access to the root directory, enable the `chroot` feature:

1. **Backup the configuration file**:

   ```bash
   cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.bak
   ```

2. **Enable chroot_local_user**:

   ```bash
   vim /etc/vsftpd/vsftpd.conf
   # Enable: chroot_local_user=YES
   ```

3. **Restart VSFTPD service**:

   ```bash
   systemctl restart vsftpd
   ```

---

## Accessing FTP Server from Client

To access the FTP server from a client, you can use various FTP clients such as lftp, gftp, FileZilla, and WinSCP.

1. **Check if FTP client is installed**:

   ```bash
   which ftp
   ```

2. **Install FTP client**:

   ```bash
   sudo yum install ftp -y
   ```

3. **Access the FTP server**:

   ```bash
   ftp server-ip-address
   ```

4. **Login with user credentials** and enter the FTP console.

5. **Useful FTP commands**:

   - `help`: Display available commands.
   - `mget test.txt`: Download the file `test.txt`.
   - `mput <file-path>`: Upload files from the client to the server.
   - `bye`: Exit the FTP session.

---

## FTP Troubleshooting

If you encounter the `oops 50x` error, it may be due to SELinux blocking the transaction. You can temporarily disable SELinux for troubleshooting:

1. **Check SELinux status**:

   ```bash
   sestatus
   ```

2. **Disable SELinux**:

   ```bash
   setenforce 0
   ```

3. **Enable SELinux again**:

   ```bash
   setenforce 1
   ```

---

## Active FTP vs Passive FTP

- **Active FTP**: Uses port 21 for connection establishment and port 20 for data transfer.
- **Passive FTP**: Uses port 21 for connection establishment and a non-privileged port (above 1024) for data transfer.

---

## Sample Configuration for VSFTPD

Here is an example of a sample configuration for `vsftpd.conf`:

```ini
listen=YES
anonymous_enable=NO
local_enable=YES
guest_enable=YES
guest_username=virtualftp
write_enable=YES
allow_writeable_chroot=YES
connect_from_port_20=YES
dirmessage_enable=YES
idle_session_timeout=600
data_connection_timeout=120

ls_recurse_enable=NO
session_support=YES
virtual_use_local_privs=YES
xferlog_file=/var/log/xferlog
vsftpd_log_file=/var/log/vsftpd.log
xferlog_std_format=YES
ftpd_banner=Welcome to Alnafi FTP Server. SSL encryption is enabled for secure data transfer.
chroot_local_user=YES
user_sub_token=$USER
pam_service_name=vsftpd
local_root=/var/ftp/$USER
hide_ids=YES

syslog_enable=YES
log_ftp_protocol=YES
xferlog_enable=YES
dual_log_enable=YES

ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=NO
force_local_logins_ssl=NO
ssl_tlsv1=YES
ssl_sslv2=YES
ssl_sslv3=YES
rsa_cert_file=/etc/ssl/vsftpd/vsftpd.pem
rsa_private_key_file=/etc/ssl/vsftpd/vsftpd.pem

pasv_min_port=20211
pasv_max_port=20218
```

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```

---

This `README.md` file is structured to provide clear, actionable steps for configuring an FTP server, securing it, accessing it from a client, and troubleshooting common issues. Adjustments can be made depending on specific system requirements or configurations.
