# Apache Web Server Installation and Virtual Host Setup Guide

This guide outlines how to install and configure Apache HTTP Server to host multiple websites (`site1` and `site2`) with SSL setup and automatic HTTP to HTTPS redirection.

## Apache HTTPD Installation Guide

### Step 1: Install Apache HTTP Server

#### For Debian/Ubuntu:

```bash
sudo apt update
sudo apt install apache2
sudo systemctl start apache2
sudo systemctl enable apache2
```

#### For CentOS/RHEL:

```bash
sudo yum update
sudo yum install httpd
sudo systemctl start httpd
sudo systemctl enable httpd
```

Verify installation with the following command:
- For Debian: `systemctl status apache2`
- For CentOS: `systemctl status httpd`

---

## Virtual Host Configuration for Multiple Sites

### 1. Create Document Roots

```bash
sudo mkdir -p /var/www/site1/public_html
sudo mkdir -p /var/www/site2/public_html
```

Create sample `index.html` files for each site:
- `/var/www/site1/public_html/index.html`
- `/var/www/site2/public_html/index.html`

### 2. Create Virtual Host Files

#### Debian/Ubuntu:

```bash
sudo nano /etc/apache2/sites-available/site1.conf
sudo nano /etc/apache2/sites-available/site2.conf
```

#### CentOS/RHEL:

```bash
sudo nano /etc/httpd/conf.d/site1.conf
sudo nano /etc/httpd/conf.d/site2.conf
```

### 3. Sample Virtual Host Configurations

**`site1.conf`**

```apache
<VirtualHost *:80>
    ServerName site1.com
    ServerAlias www.site1.com
    DocumentRoot /var/www/site1/public_html
    ErrorLog ${APACHE_LOG_DIR}/site1_error.log
    CustomLog ${APACHE_LOG_DIR}/site1_access.log combined

    # Redirect to HTTPS
    Redirect permanent / https://site1.com/
</VirtualHost>

<VirtualHost *:443>
    ServerName site1.com
    ServerAlias www.site1.com
    DocumentRoot /var/www/site1/public_html
    ErrorLog ${APACHE_LOG_DIR}/site1_ssl_error.log
    CustomLog ${APACHE_LOG_DIR}/site1_ssl_access.log combined

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
    SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
</VirtualHost>
```

**`site2.conf`**

```apache
<VirtualHost *:80>
    ServerName site2.com
    ServerAlias www.site2.com
    DocumentRoot /var/www/site2/public_html
    ErrorLog ${APACHE_LOG_DIR}/site2_error.log
    CustomLog ${APACHE_LOG_DIR}/site2_access.log combined

    # Redirect to HTTPS
    Redirect permanent / https://site2.com/
</VirtualHost>

<VirtualHost *:443>
    ServerName site2.com
    ServerAlias www.site2.com
    DocumentRoot /var/www/site2/public_html
    ErrorLog ${APACHE_LOG_DIR}/site2_ssl_error.log
    CustomLog ${APACHE_LOG_DIR}/site2_ssl_access.log combined

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
    SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
</VirtualHost>
```

### 4. Enable Sites (Debian/Ubuntu)

```bash
sudo a2ensite site1.conf
sudo a2ensite site2.conf
sudo systemctl reload apache2
```

---

## SSL Implementation

### 1. Generate Self-Signed Certificate

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/ssl/private/apache-selfsigned.key \
-out /etc/ssl/certs/apache-selfsigned.crt
```

### 2. Enable SSL Module

#### Debian/Ubuntu:

```bash
sudo a2enmod ssl
```

#### CentOS/RHEL:

```bash
sudo yum install mod_ssl
```

---

## Final Steps

1. **Test configuration:**

   ```bash
   sudo apachectl configtest
   ```

2. **Reload Apache:**

   - For Debian/Ubuntu:
     ```bash
     sudo systemctl reload apache2
     ```

   - For CentOS/RHEL:
     ```bash
     sudo systemctl reload httpd
     ```

3. **Configure firewall to allow HTTP and HTTPS traffic:**

   ```bash
   sudo ufw allow 80/tcp
   sudo ufw allow 443/tcp
   ```

---

## Notes

- This setup configures two virtual hosts (`site1` and `site2`) with automatic redirection from HTTP to HTTPS.
- The SSL certificates are self-signed. For production environments, it is recommended to use CA-signed certificates (e.g., from [Let's Encrypt](https://letsencrypt.org)).
- Ensure your DNS settings are correctly configured for `site1.com` and `site2.com`.

---

This guide provides step-by-step instructions for installing Apache, setting up virtual hosts, implementing SSL, and ensuring HTTP to HTTPS redirection for two websites.
