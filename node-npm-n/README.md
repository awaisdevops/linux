# Node.js and NPM Management on Ubuntu 20.04

This guide explains how to install Node.js, npm, and various package managers (n, nvm) on Ubuntu 20.04, manage multiple versions of Node.js, and how to completely uninstall them.

## Installing Node.js on Ubuntu 20.04

### Step 1: Install Node.js 20

To install Node.js 20, follow these steps:

1. Update your system's package list:

   ```bash
   sudo apt-get update -y
   ```

2. Install required dependencies:

   ```bash
   sudo apt-get install curl gnupg2 -y
   ```

3. Download and install Node.js 14 setup script:

   ```bash
   curl -sL https://deb.nodesource.com/setup_14.x | bash -
   ```

4. Update package list again:

   ```bash
   sudo apt-get update -y
   ```

5. Install Node.js:

   ```bash
   sudo apt-get install nodejs -y
   ```

6. Verify the installation:

   ```bash
   node -v
   npm -v
   ```

> **Note:** Installing Node.js also installs npm (Node Package Manager) automatically.

### Step 2: Update npm to Latest Version

To update npm to the latest version, run the following commands:

```bash
npm install npm@latest -g
npm install npm@7.19.1 -g
npm install npm@10.2.5 -g
```

### Step 3: Clear Node Cache

To clear the npm cache, run:

```bash
rm -rf /.nvm && rm -rf /.npm && rm -rf ~/.bower
```

Run this command for both root and user accounts.

---

## Managing Node.js Versions with `n` and `nvm`

### Introduction

You can manage multiple Node.js versions on Ubuntu using two popular version managers:

1. **n** - A globally installed version manager.
2. **nvm** - A profile-based version manager.

#### Installing `n`

To install `n`, use one of the following methods:

**Method 1: Using npm**

```bash
npm install -g n
```

**Method 2: Using a script**

```bash
curl -L https://git.io/n-install | bash
source ~/.bashrc
```

To uninstall `n`, run:

```bash
n-uninstall
```

To update `n` to the latest version:

```bash
n-update
```

To check the version of `n`:

```bash
n --version
```

To list available commands and options for `n`:

```bash
n --help
```

#### Managing Node.js Versions with `n`

To install various Node.js versions, use the following commands:

- Install the latest Node.js version:

  ```bash
  n latest
  ```

- Install the LTS (Long Term Support) version:

  ```bash
  n lts
  ```

- Install a specific Node.js version (e.g., 14.15.0):

  ```bash
  n 14.15.0
  ```

- Download a specific Node.js version (e.g., 14.8.0):

  ```bash
  n -d 14.8.0
  ```

- Use the downloaded Node.js version:

  ```bash
  n run 14.8.0
  ```

- Exit the Node.js shell:

  ```bash
  .exit
  ```

- List installed Node.js versions:

  ```bash
  n ls
  ```

- List all available versions:

  ```bash
  n ls-remote
  ```

- List all available Node.js versions (with `--all` flag):

  ```bash
  n ls-remote --all
  ```

- Find the location of a specific Node.js version:

  ```bash
  n which node/14.8.0
  ```

- Switch between Node.js versions interactively:

  ```bash
  n
  ```

  You’ll be prompted to select a version using the up/down arrow keys.

#### Removing Node.js Versions with `n`

- To remove a specific Node.js version:

  ```bash
  n rm node/16.4.0
  ```

- To remove all downloaded Node.js versions:

  ```bash
  n prune
  ```

- To remove all installed Node.js versions:

  ```bash
  n uninstall
  ```

---

## Installing NVM (Node Version Manager)

To install `nvm`, follow these steps:

1. Run the installation script:

   ```bash
   curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
   ```

2. Close and reopen your terminal, or run the following commands to use `nvm` immediately:

   ```bash
   export NVM_DIR="$HOME/.nvm"
   [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
   [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
   ```

3. Verify the installation of `nvm`:

   ```bash
   nvm --version
   ```

   You should see an output like:

   ```
   0.34.0
   ```

### Installing Node.js with NVM

1. Install Node.js:

   ```bash
   nvm install node
   nvm install --lts
   ```

2. Verify the installation:

   ```bash
   node --version
   ```

   The output should show the installed Node.js version, e.g., `v12.8.1`.

3. Verify npm installation:

   ```bash
   npm -v
   ```

   The output should show the installed npm version, e.g., `6.13.4`.

---

## Completely Uninstall Node.js, npm, and Node Versions

To completely remove Node.js, npm, and all Node versions, follow these steps:

1. Remove Node.js and npm:

   ```bash
   sudo apt-get remove nodejs npm node
   sudo apt-get purge nodejs
   ```

2. Remove related files and directories:

   ```bash
   sudo rm -rf /usr/local/bin/npm
   sudo rm -rf /usr/local/share/man/man1/node*
   sudo rm -rf /usr/local/lib/dtrace/node.d
   sudo rm -rf ~/.npm
   sudo rm -rf ~/.node-gyp
   sudo rm -rf /opt/local/bin/node
   sudo rm -rf /opt/local/include/node
   sudo rm -rf /opt/local/lib/node_modules
   sudo rm -rf /usr/local/lib/node*
   sudo rm -rf /usr/local/include/node*
   sudo rm -rf /usr/local/bin/node*
   ```

3. Clear Node cache:

   ```bash
   rm -rf /.nvm && rm -rf /.npm && rm -rf ~/.bower
   ```

   Run this command for both root and user accounts.

---

## Useful Node and npm Commands

- To check the Node.js version:

  ```bash
  sudo node -v
  ```

- To check the npm version:

  ```bash
  sudo npm -v
  ```

- To check the installed versions with `n`:

  ```bash
  sudo n ls
  ```

- To check available Node versions for download:

  ```bash
  sudo n --all lsr
  ```

- To switch Node versions with `n`:

  ```bash
  sudo n {version-number}  # Example: sudo n 16.15.1
  ```

- To install a package via npm:

  ```bash
  sudo npm install {package-name}
  ```

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
