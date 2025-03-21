# Composer Installation and Version Switcher

This guide explains how to install multiple versions of Composer on a Linux system and how to switch between them.

## Steps to Install Composer

### Step 1: Download the Composer Installer

First, download the Composer installer using the `wget` command:

```bash
wget -O composer-setup.php https://getcomposer.org/installer
```

### Step 2: Install Composer Globally

Once the installer is downloaded, run the following command to install Composer globally:

```bash
php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

You should see the following output:

```
All settings correct for using Composer
Downloading...

Composer (version 2.2.6) successfully installed to: /usr/local/bin/composer
Use it: php /usr/local/bin/composer
```

### Step 3: Verify the Composer Installation

Once the installation is complete, verify the Composer version with the following command:

```bash
composer -V
```

The output should show something like:

```
Composer version 2.2.6 2022-02-04 17:00:38
```

### Step 4: Install Composer Locally (Optional)

If you want to install Composer locally for your PHP project, run:

```bash
php composer-setup.php --install-dir=/your-php-project
```

### Step 5: Update Composer

To update Composer to the latest version, run the following command:

```bash
composer self-update
```

If you already have the latest version, you'll see output like this:

```
You are already using the latest available Composer version 2.2.6 (stable channel)
```

---

## Switching Composer Versions

### Switch to Composer Version 1

To switch to Composer version 1, run the following command:

```bash
composer self-update --1
```

Once version 1 is installed, you can install your project dependencies.

### Switch Back to Composer Version 2

To switch back to Composer version 2, run:

```bash
composer self-update --2
```

### Switch to a Specific Composer Version

You can switch to a specific version by passing the version number. For example, to switch to version `1.10.22` or `2.1.3`, run the following commands:

```bash
composer self-update 1.10.22
composer self-update 2.1.3
```

### Revert to the Previous Version

If you want to revert to the previous version of Composer, run:

```bash
composer self-update --rollback
```

### Preview Composer Version

To install a pre-release version, use the following command:

```bash
composer self-update --preview
```

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
