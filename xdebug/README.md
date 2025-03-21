# Xdebug Installation Guide

This guide provides the steps to install Xdebug for a specific PHP version using `phpbrew`. Follow the steps below to install and configure Xdebug on your system.

## Prerequisites

Ensure that the correct PHP version is installed and set up using `phpbrew`. You need to confirm the PHP version required for your environment.

### Step 1: Confirm PHP Version

1. To check the currently installed PHP version, run:
    ```bash
    php -v
    ```

2. List all the PHP versions managed by `phpbrew`:
    ```bash
    phpbrew list
    ```

3. If the desired PHP version (e.g., PHP 7.4.3) is not active, switch to it:
    ```bash
    phpbrew switch php-7.4.3
    ```

4. Update Apache to use the desired PHP version:
    ```bash
    set_apache_php 7.4
    ```

5. Restart Apache to apply changes:
    ```bash
    systemctl restart apache2
    ```

6. Verify the PHP version by navigating to the user IP in your browser:
    ```bash
    http://172.16.0.23/info.php
    ```

7. Check the version set to ensure it is correct (e.g., PHP 7.4.3).

---

## Step 2: Verify Xdebug Compatibility with PHP Version

1. Visit the following link to check Xdebug compatibility with your required PHP version:
   [Xdebug Download Historical Versions](https://xdebug.org/download/historical)

2. Under "Xdebug x.x.x" (not "Xdebug 3.2.0alpha2" or "Xdebug 3.2.0RC2"), choose the compatible version for your PHP.

3. Copy the link for the compatible Xdebug version.

---

## Step 3: Download and Install Xdebug

1. Change to the `/opt/` directory:
    ```bash
    cd /opt/
    ```

2. Download the compatible Xdebug version:
    ```bash
    wget https://xdebug.org/files/xdebug-3.0.2.tgz
    ```

3. Extract the downloaded tarball:
    ```bash
    tar -xvzf xdebug-3.0.2.tgz
    ```

4. Navigate to the extracted Xdebug directory:
    ```bash
    cd xdebug-2.6.0
    ```

5. Prepare for compilation with `phpize`:
    ```bash
    phpize
    ```

6. Configure and install Xdebug:
    ```bash
    ./configure
    make
    sudo make install
    ```

---

## Step 4: Configure PHP to Use Xdebug

After installation, Xdebug will be installed and you need to link it to the PHP configuration.

1. To find the PHP configuration file (`php.ini`), run:
    ```bash
    php --ini
    ```

2. Locate the `php.ini` file for your installed PHP version (e.g., `/opt/phpbrew/php/php-7.4.20/etc/php.ini`).

3. Add the following Xdebug configuration to the `php.ini` file:
    ```ini
    [xdebug]
    zend_extension = /opt/phpbrew/php/php-7.4.9/lib/php/extensions/no-debug-zts-20190902/xdebug.so
    xdebug.remote_enable=1
    xdebug.profiler_output_dir="/tmp/xdebug/"
    xdebug.profiler_enable=On
    xdebug.remote_host="127.0.0.1"
    xdebug.remote_port=9001
    xdebug.remote_handler="dbgp"
    xdebug.remote_connect_back=1
    xdebug.idekey="VSCODE"
    ```

4. Set the following additional PHP configurations if not already set:
    ```ini
    memory_limit = 512M
    max_input_time = 600
    max_execution_time = 1800
    upload_max_filesize = 200M
    post_max_size = 400M
    max_file_uploads = 40
    date.timezone = "UTC"
    error_reporting = E_ALL & E_DEPRECATED & E_STRICT & E_WARNING & E_NOTICE
    display_errors = Off
    expose_php = Off
    ```

---

## Step 5: Verify Xdebug Installation

1. After completing the above steps, restart your Apache server to apply changes:
    ```bash
    systemctl restart apache2
    ```

2. Verify that Xdebug is installed and running by using the `php -m` command to check if Xdebug appears in the list of PHP modules:
    ```bash
    php -m | grep xdebug
    ```

---

## Troubleshooting

- If you encounter issues with Xdebug, ensure that the PHP version and Xdebug version are compatible.
- Ensure that the correct path to `xdebug.so` is used in the `zend_extension` directive in the `php.ini` file.

---

## Conclusion

You have now successfully installed and configured Xdebug for a specific PHP version. You can use Xdebug for debugging and profiling your PHP applications.
