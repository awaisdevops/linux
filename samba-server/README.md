# Guide to Install and Configure Samba Server on Ubuntu 20.04

This guide walks you through the steps required to install and configure a **Samba server** on Ubuntu 20.04. Samba is a software suite that enables file and print sharing between computers running Linux and Windows.

## Step 1: Update the System

First, ensure your system is up to date. Run the following commands:

```bash
sudo apt update
```

## Step 2: Install Samba

Install Samba and its dependencies by running:

```bash
sudo apt install samba -y
```

## Step 3: Verify the Installation

After the installation is complete, verify that the Samba service is running:

```bash
sudo systemctl status smbd
```

The output should show that the **smbd** service is active and running.

## Step 4: Configure Samba Server

The main Samba configuration file is located at `/etc/samba/smb.conf`. Before modifying it, it’s a good idea to back up the original configuration.

### Backup the Configuration File

```bash
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
```

### Edit the Configuration File

Open the Samba configuration file using a text editor:

```bash
sudo nano /etc/samba/smb.conf
```

At the bottom of the file, add the following lines to configure a shared folder:

```bash
[Shared]
   path = /srv/samba/shared
   browseable = yes
   read only = no
   guest ok = yes
```

This will create a shared folder named **Shared** located at `/srv/samba/shared`, and it will allow anyone (guest access) to read and write to the folder.

### Save and Close

After making your changes, save the file by pressing `CTRL + X`, then `Y` to confirm, and `Enter` to save the file.

## Step 5: Create the Shared Directory

Create the shared directory and set proper permissions:

```bash
sudo mkdir -p /srv/samba/shared
sudo chmod 2775 /srv/samba/shared
sudo chown nobody:nogroup /srv/samba/shared
```

This will create the directory with the necessary permissions for Samba to use.

## Step 6: Restart Samba Service

Restart the Samba service to apply the changes:

```bash
sudo systemctl restart smbd
```

## Step 7: Allow Samba Through the Firewall

If you're using **UFW**, you'll need to allow Samba traffic. Run the following command to allow Samba:

```bash
sudo ufw allow samba
```

## Step 8: Add Samba User

You can add a Samba user for authentication. If you already have a Linux user (e.g., `username`), you can add that user to Samba by running:

```bash
sudo smbpasswd -a username
```

You will be prompted to set a password for the Samba user. Afterward, enable the user:

```bash
sudo smbpasswd -e username
```

## Step 9: Test the Samba Configuration

To ensure that the Samba server is configured correctly, run the following command:

```bash
testparm
```

If there are no errors, the configuration is valid.

## Step 10: Access the Samba Share from a Windows or Linux Machine

### From Windows

- Open **File Explorer**.
- In the address bar, type `\\<server-ip>\Shared` and press **Enter** (replace `<server-ip>` with your server’s IP address).
- You should see the shared folder, and you’ll be prompted for a Samba username and password if you've set up one.

### From Linux

- Open **File Manager** (Nautilus, Thunar, etc.).
- In the address bar, type `smb://<server-ip>/Shared` and press **Enter** (replace `<server-ip>` with your server’s IP address).
- You should see the shared folder and be able to access its contents.

## Step 11: Configure Samba to Start Automatically on Boot

Ensure that the Samba service starts automatically on boot:

```bash
sudo systemctl enable smbd
```

---

## Additional Tips

- **User Permissions**: You can configure more advanced permissions and security settings in the Samba configuration file, including restricting access to certain users or IP addresses.
- **Samba Shares for Specific Users**: To create shares accessible only to specific users, you can modify the `[Shared]` section to specify the allowed users with the `valid users` directive.

```bash
[Shared]
   path = /srv/samba/shared
   valid users = username
   browseable = yes
   read only = no
   guest ok = no
```

- **Access Samba Logs**: Samba logs are typically stored in `/var/log/samba/`. Check these logs if you encounter any issues.

This README.md file provides all the necessary instructions for installing and configuring a Samba server on Ubuntu 20.04, along with some additional tips for managing the server.
