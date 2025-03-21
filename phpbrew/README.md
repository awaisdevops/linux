# Installing and Managing Multiple PHP Versions with PHPBrew on Linux

This guide provides detailed steps on how to install multiple versions of PHP using PHPBrew and manage them on a Linux system.

## Table of Contents

- [Install or Update PHPBrew](#install-or-update-phpbrew)
- [Remove PHPBrew](#remove-phpbrew)
- [Useful PHPBrew Commands](#useful-phpbrew-commands)
- [Installing and Switching PHP Versions](#installing-and-switching-php-versions)
- [Setting Up Apache with PHPBrew](#setting-up-apache-with-phpbrew)
- [Verifying PHP Versions](#verifying-php-versions)
- [Managing PHP Extensions](#managing-php-extensions)
- [Completely Purging PHP](#completely-purging-php)
- [Troubleshooting](#troubleshooting)

---

## Install or Update PHPBrew

**Install PHPBrew** (requires one PHP version already installed):
   ```bash
   curl -L -O https://github.com/phpbrew/phpbrew/releases/latest/download/phpbrew.phar
   chmod +x phpbrew.phar
   sudo mv phpbrew.phar /usr/local/bin/phpbrew
   phpbrew init
   phpbrew --version
   ```

**Set up the environment variables for PHPBrew**:
   ```bash
   vi ~/.bashrc
   source /root/.phpbrew/bashrc
   source ~/.bashrc
   reboot system now and check the version.
   ```

---

## Remove PHPBrew

To completely remove PHPBrew from your system, run the following commands:
```bash
rm -rf ~/.phpbrew/
rm -rf /usr/local/bin/phpbrew
vim ~/.bashrc  # Remove the environment variables for PHPBrew
```

---

## Useful PHPBrew Commands

- **To check the PHP version**:
  ```bash
  php -v
  ```

- **To check the path of `php.ini`**:
  ```bash
  php -ini | grep Loaded
  ```

- **To list installed PHP extensions**:
  ```bash
  php -m
  ```

- **To check the path of installed PHP extensions**:
  ```bash
  php -i | grep extension_dir
  ```

- **To list installed PHP versions**:
  ```bash
  phpbrew list
  ```

- **To check available versions for installation**:
  ```bash
  phpbrew known
  ```

- **To update available versions**:
  ```bash
  phpbrew update
  ```

- **To switch PHP version**:
  ```bash
  phpbrew switch php-VersionNumber
  ```

- **To load the respective PHP version in Apache**:
  ```bash
  sudo set_apache_php {5.5, 5.6, 7.1, 7.2, 7.3, 7.4, 8.0, etc.}
  ```

- **To install a PHP extension**:
  ```bash
  phpbrew extension install {Extension_Name}
  ```

---

## Installing and Switching PHP Versions

### Installing a New PHP Version with Extensions

Use the following command to install PHP 8.2.1 with various extensions:
```bash
phpbrew install 8.2.1 +apxs2 +bcmath +bz2 +calendar +ctype +curl +dom +exif +fileinfo +filter +ftp +gd +gettext +hash +iconv +imap +inifile +inline +json +kerberos +libgcc +mbregex +mbstring +mcrypt +mhash +mysql +openssl +session +soap +sockets +sqlite +xml +xmlrpc +zip +zlib +zts +pdo -- --with-kerberos --with-imap-ssl --with-gd=shared --enable-gd-natf --with-jpeg-dir=/usr --with-png-dir=/usr --with-webp-dir=/usr
```

If you face an error related to `imap`, remove `imap` and `--with-imap-ssl` from the above command and run the following commands to install `imap`, `gd`, and `intl` extensions manually:
```bash
phpbrew ext install imap -- --with-kerberos --with-imap-ssl
phpbrew ext install gd
phpbrew ext install intl
```

---

## Setting Up Apache with PHPBrew

### Change Apache PHP Version

To switch the PHP version for Apache, follow these steps:

1. Navigate to `/usr/lib/apache2/modules/`:
   ```bash
   cd /usr/lib/apache2/modules/
   ls -ltr
   ```

2. Rename the `libphp.so` file:
   ```bash
   mv libphp.so libphp8.2.1.so
   ls -ltr
   ```

3. Backup the `set_apache_php` script and modify it:
   ```bash
   cp /bin/set_apache_php /bin/set_apache_php-bak
   ls -ltr /bin/set_apache_php*
   vim /bin/set_apache_php
   ```

   Use the following script to switch PHP versions in Apache:
   ```bash
   #!/bin/bash
   if [[ $1 =~ 5 ]]; then
       sudo a2enmod php5 > /dev/null 2>&1;
       sudo a2dismod php7 > /dev/null 2>&1;
       sudo a2dismod php8 > /dev/null 2>&1;
       if [[ $1 =~ 5.5 ]]; then
           echo 'LoadModule php5_module        /usr/lib/apache2/modules/libphp5.5.38.so' | sudo tee /etc/apache2/mods-available/php5.load
       elif [[ $1 =~ 5.6 ]]; then
           echo 'LoadModule php5_module        /usr/lib/apache2/modules/libphp5.6.40.so' | sudo tee /etc/apache2/mods-available/php5.load
       elif [[ $1 =~ 5.4 ]]; then
           echo 'LoadModule php5_module        /usr/lib/apache2/modules/libphp5.4.45.so' | sudo tee /etc/apache2/mods-available/php5.load
       fi
       sudo service apache2 restart;
   elif [[ $1 =~ 7 ]]; then
       sudo a2enmod php7 > /dev/null 2>&1;
       sudo a2dismod php5 > /dev/null 2>&1;
       sudo a2dismod php8 > /dev/null 2>&1;
       if [[ $1 =~ 7.1 ]]; then
           echo 'LoadModule php7_module        /usr/lib/apache2/modules/libphp7.1.33.so' | sudo tee /etc/apache2/mods-available/php7.load
       elif [[ $1 =~ 7.2 ]]; then
           echo 'LoadModule php7_module        /usr/lib/apache2/modules/libphp7.2.34.so' | sudo tee /etc/apache2/mods-available/php7.load
       elif [[ $1 =~ 7.3 ]]; then
           echo 'LoadModule php7_module        /usr/lib/apache2/modules/libphp7.3.28.so' | sudo tee /etc/apache2/mods-available/php7.load
       elif [[ $1 =~ 7.4 ]]; then
           echo 'LoadModule php7_module        /usr/lib/apache2/modules/libphp7.4.20.so' | sudo tee /etc/apache2/mods-available/php7.load
       fi
       sudo service apache2 restart;
   elif [[ $1 =~ 8 ]]; then
       sudo a2enmod php8 > /dev/null 2>&1;
       sudo a2dismod php5 > /dev/null 2>&1;
       sudo a2dismod php7 > /dev/null 2>&1;
       if [[ $1 =~ 8.0 ]]; then
           echo 'LoadModule php_module        /usr/lib/apache2/modules/libphp8.0.30.so' | sudo tee /etc/apache2/mods-available/php8.load
       elif [[ $1 =~ 8.1 ]]; then
           echo 'LoadModule php_module        /usr/lib/apache2/modules/libphp8.1.27.so' | sudo tee /etc/apache2/mods-available/php8.load
       elif [[ $1 =~ 8.2 ]]; then
           echo 'LoadModule php_module        /usr/lib/apache2/modules/libphp8.2.16.so' | sudo tee /etc/apache2/mods-available/php8.load
       elif [[ $1 =~ 8.3 ]]; then
           echo 'LoadModule php_module        /usr/lib/apache2/modules/libphp8.3.3.so' | sudo tee /etc/apache2/mods-available/php8.load
       fi
       sudo service apache2 restart;
   fi
   ```

4. Modify the `php8.load` and `php8.conf` files in `/etc/apache2/mods-available/`:
   ```bash
   cd /etc/apache2/mods-available/
   ls -ltr
   mv php.load php8.load
   ls -ltr
   vim php8.load
   ```

   Update the `php8.load` file with:
   ```bash
   LoadModule php_module        /usr/lib/apache2/modules/libphp8.2.1.so
   ```

   Modify the `php8.conf` file with:
   ```bash
   <FilesMatch ".+\.ph(ar|p|tml)$">
       SetHandler application/x-httpd-php
   </FilesMatch>
   <FilesMatch ".+\.phps$">
       SetHandler application/x-httpd-php-source
       Require all denied
   </FilesMatch>
   <FilesMatch "^\.ph(ar|p|ps|tml)$">
       Require all denied
   </FilesMatch>
   <IfModule mod_userdir.c>
       <Directory /home/*/public_html>
           php_admin_flag engine Off
       </Directory>
   </IfModule>
   ```

5. Make the `php8.load` and `php8.conf` files executable:
   ```bash
   chmod +x php8.load
   chmod +x php8.conf
   ```

---

## Verifying PHP Versions

1. **Switch PHP version**:
   ```bash
   phpbrew switch php-8.2.1 && set_apache_php 8.2 && php -v
   ```

2. **Verify the PHP version**:
   ```bash
   php -v && set_apache_php
   ```

---

## Managing PHP Extensions

### Install LDAP Extension:
```bash
phpbrew ext install ldap
```

### Install Sodium Extension:
To install PHP-Sodium extension using `phpbrew`, run:
```bash
phpbrew -vvv ext install libsodium
```

If `libsodium` is not installed at the system level, install it manually:
```bash
./configure
make && make check
sudo make install
```

### Install SSH2 Extension:

1. Clone the `pecl-networking-ssh2` repository:
   ```bash
   cd /opt
   git clone https://github.com/php/pecl-networking-ssh2.git
   cd pecl-networking-ssh2
   phpize
   ./configure
   make
   make install
   ```

2. Enable the SSH2 extension in `php.ini`:
   ```bash
   php --ini
   vim path-to-ini-file
   ```

   Add:
   ```ini
   extension="ssh2.so"
   ```

3. Verify the extension:
   ```bash
   php -m | grep -i ssh2
   ```

---

## Completely Purging PHP

To completely remove PHP, run:
```bash
sudo apt-get remove --purge php*
sudo apt-get purge php*
sudo apt-get autoremove
sudo apt-get autoclean
```

---

## Troubleshooting

### Error: "Your system is not ready to run the application"
This error indicates that PHPBrew 2 is not compatible with PHP versions 8 and above. To resolve it, downgrade PHPBrew.

### PHPBrew Update Issues
If `phpbrew update` doesn’t work, try installing CA certificates:
```bash
sudo apt-get update && sudo apt-get install -y ca-certificates
```

If this doesn't resolve the issue, upgrade PHPBrew:
```bash
phpbrew update
```

---

## Removing PHP Versions

To remove a specific PHP version:
```bash
phpbrew remove php-8.0.30
```

To remove PHPBrew:
```bash
phpbrew remove <php-version-to-remove>
```

---

This `README.md` contains all the instructions to install, configure, and manage PHP versions using PHPBrew, along with troubleshooting steps, commands for managing PHP extensions, and Apache configuration.
