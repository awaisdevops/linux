# SSH Protocol (Secure Shell)

SSH (Secure Shell) is used for remote login and it operates on port 22. SSH communication is encryption-protected, unlike Telnet which sends data in plaintext and is susceptible to attacks.

## Check SSH Installation on CentOS 7
To check if SSH is installed and running on your CentOS 7 server:

```bash
rpm -qa | grep -i ssh
# Check if OpenSSH server is installed

systemctl status sshd
# Check the status of the SSH service

systemctl enable sshd
# Enable SSH service to start on boot

systemctl restart sshd
# Restart SSH service

systemctl status sshd
# Verify if SSH service is active

netstat -tulnp | grep -i 22
# Check for processes using port 22 (default SSH port)

ss -tulnp | grep -i 22
# Alternative to netstat to check for processes using port 22

telnet 127.0.0.1 22
# Test if SSH port (22) is open locally
```

## SSH Configuration Files
- The SSH server configuration file is located at `/etc/ssh/sshd_config`.
- The client-side configuration file is located at `/etc/ssh/ssh_config`.

## Connect to a Server Using SSH
To connect to a server via SSH:

```bash
ssh root@server1
# Connect as root user to server1

ssh server1 -l wwe
# Connect to server1 as user 'wwe'

ssh server1 -p 22
# Explicitly specify the SSH port (default is 22)
```

## How SSH Connects to the Server on Port 22
SSH connects to the server through port 22 (by default). This port number is defined in the `/etc/services` file. If the SSH port is changed (e.g., to 2222), SSH will try to connect on that port. If the server is still running SSH on port 22, the connection will be refused.

### Change SSH Port Number
To change the SSH port from the default (22) to a custom port (e.g., 9495):

1. Edit the SSH configuration file:

```bash
vim /etc/ssh/sshd_config
# Open the SSH configuration file and change the port number from 22 to 9495
```

2. Add the new port to the firewall:

```bash
firewall-cmd --permanent --add-port=9495/tcp
firewall-cmd --reload
firewall-cmd --list-ports
# Verify that port 9495 is open
```

3. Allow the new port in SELinux:

```bash
semanage port -a -t ssh_port_t -p tcp 9495
```

4. Restart the SSH service:

```bash
systemctl restart sshd
```

5. Connect using the new port:

```bash
ssh server1 -l wwe -p 9495
# Connect to the server at the custom port 9495
```

## Disabling SSH Root Login
To disable root login via SSH:

1. Edit the SSH configuration file:

```bash
vi /etc/ssh/sshd_config
# Change PermitRootLogin to 'no'
```

2. Restart the SSH service:

```bash
systemctl restart sshd
```

## Generate SSH Key Pair
To generate an SSH key pair:

```bash
ssh-keygen -t rsa -b 4096
# Generate a public and private RSA key with a key length of 4096 bits
```

To copy your SSH public key to a remote server:

```bash
ssh-copy-id user@remote_host
# Copy your public key to the remote server

ssh-copy-id -i /path/to/your/public/key.pub -p port_number user@remote_host
# Use a custom path for the public key and specify a custom SSH port
```

To use the SSH key with a specific server:

```bash
ssh -i /home/john-cena/Downloads/key -p 2245 root@dambezuma.com
# Connect to the server using a specific private key and port
```

---

## License
MIT License
```

This `README.md` document covers all the SSH protocol-related tasks, commands, and explanations provided in the guide.
