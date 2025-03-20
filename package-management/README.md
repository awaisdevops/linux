# Software Installation / Package Management Guide

## Installation and Uninstallation

Installation refers to writing files into different file systems on the hard disk. The removal of these data files from those file systems is termed uninstallation.

## DEB vs RPM

DEB and RPM are two software package file extensions used by the apt and yum package managers on Linux OS. 

- **DEB**: Used by apt package manager-based Linux distros.
- **RPM**: Used by yum package manager-based Linux distros.

We can install packages in 4 ways:
1. Manual install using RPM.
2. Using yum package manager.
3. Source code installation/tarball compilation.
4. SRPM (Source RPM).

## Can we install Windows `.exe` files in Linux?

No, packages having Windows `.exe` extensions can't be installed in any Linux distro by default. However, we have workarounds using Wine (open-source) or Crossover (commercial) software. These emulators are not suitable for production but can be used for R&D purposes. They can only handle small packages, and larger `.exe` packages tend to crash when installed using Wine or Crossover.

## 1. Manual Package Installation

We can get the package from an ISO DVD or download the package from the internet and install it.

### Mounting DVD

We mount the CentOS DVD ISO in VirtualBox and VMware Workstation.

```bash
df -h
```

This will auto-mount the DVD.

If the DVD doesn't show, we can scan it for host1, host2, etc.

```bash
echo "- - -" > /sys/class/scsi_host/host0/scan
```

Then, mount the DVD.

```bash
mount /dev/cdrom /mnt/
```

If we face DVD mount errors, we can mount the CD-ROM driver to `/mnt/`.

### RPM Installation

RPM stands for Red Hat Package Manager. It maintains a flat DB file that stores information about all packages.

```bash
/var/lib/rpm
```

Here, we have a file named `Packages` that contains information about all the packages downloaded or installed. All RPM commands fetch info from here.

```bash
rpm --rebuilddb
```

This command rebuilds the RPM database.

To check what files will be installed and where:

```bash
rpm -qlp zsh-4.3.11-8.el6.centos.x86_64.rpm
rpm -qlp zsh-4.3.11-8.el6.centos.x86_64.rpm | wc -l
```

To install the package:

```bash
rpm -ivh zsh-4.3.11-8.el6.centos.x86_64.rpm
```

`ivh` stands for Install Verbose Hash, and hash is a database file that converts junk into hashes.

To install without dependencies:

```bash
rpm -ivh --nodeps package_name----.rpm
```

This installs the package without dependencies, but it may throw an error. This flag can be used when we can ignore files like documentation, but not vital files like `.so` libraries.

To forcefully install the package:

```bash
rpm -ivh --force zsh-4.3.11-8.el6.centos.x86_64.rpm
```

To check the number of files written to the hard disk:

```bash
rpm -ql zsh
rpm -ql zsh | wc -l
```

To search for installed packages:

```bash
rpm -qa | grep -i zsh
```

To get info about the package:

```bash
rpm -qi zsh
```

To remove the package:

```bash
rpm -ev zsh
```

### Upgrade the Package

To upgrade a package:

```bash
rpm -Uvh zsh-5.7-3.1.x86_64.rpm
```

Use the `U` flag to upgrade the package and confirm.

## 2. Using Yum Package Manager

Yum stands for Yellow Dog Updater Modifier and is the CentOS/RedHat/Fedora package manager. Yum downloads and installs packages and all their dependencies using the `rpm -ivh` command.

### How Yum Works

When we execute:

```bash
yum install docker
```

Yum refers to repository files at `/etc/yum.repos.d/`. It checks for the repository server URL that has our required package and is geographically nearest to the user using HTTP/FTP protocol. After consulting the `repomd.xml` config file, it downloads the required package and its dependencies.

Packages are temporarily downloaded to:

```bash
/var/cache/yum/x86_64/7/base/packages
```

After a successful installation, the downloaded package is removed from this directory.

### Yum Troubleshooting

If yum doesn't work, check the following:

```bash
1: Check /etc/resolv.conf
2: Check if HTTP/HTTPS ports are allowed in the firewall for your server
3: Kill the yum process
4: Clean yum cache using 'yum clean all'
```

### Famous Yum Repositories

- Fedora's EPEL
- Remi
- Dag
- rpm.pbone.net
- Webtatic

### How to Add EPEL Repo

Before installing a package, use the following to check if the package exists in the repositories:

```bash
yum search zsh
yum search xmms
```

If the package doesn't exist, you can add Fedora's EPEL repository:

```bash
yum install epel-release
```

Confirm installation:

```bash
rpm -qa | grep -i epel
yum repolist
```

To install a package from the EPEL repo:

```bash
yum --enablerepo=epel install xmms
```

### How to Install and Enable Remi Repository in RHEL/CentOS/Rocky 6/7/8

Remi’s RPM repository is a free and stable YUM repository, mainly for the PHP stack.

```bash
yum install epel-release
wget https://rpms.remirepo.net/enterprise/remi-release-7.rpm
rpm -ivh remi-release-7.rpm
yum repolist
yum repolist | grep -i remi
```

### Important Yum Commands

- `yum install <package-name-to-install>`
- `yum update <package-name-to-update>`
- `rpm -qa | grep -i <package-name-to-check-if-installed>`
- `yum search <package-name-to-search>`
- `yum info <package-name-to-check-its-info>`
- `yum remove <package-name-to-remove>`
- `yum -e <package-name-to-erase>`
- `yum repolist` (To print all the repositories)
- `yum --help` (To print useful yum flags)

## Configuring Yum Local Repository Server or Yum Offline Installation

### Steps for Setting Up a Local Repository

1. Mount the CentOS 7 ISO DVD to your VirtualBox or VMware Workstation.
   
   ```bash
   df -h
   ```

2. Scan the host for DVD:

   ```bash
   echo '- - -' > /sys/class/scsi_host/host0/scan
   ```

3. Copy all `.rpm` packages to `/data`:

   ```bash
   rsync -parv /run/media/root/CentOS\ 7\ x86_64/Packages* /data/
   ```

4. Create a `repomd.xml` file:

   ```bash
   createrepo -v /data/Packages/
   ```

5. Enable the local repository by creating a new repo file:

   ```bash
   cd /etc/yum.repos.d/
   vim Local.repo
   ```

   Add the following:

   ```ini
   [Local]
   name=[Local]
   baseurl=file:///data/Packages/
   enabled=1
   gpgcheck=0
   ```

   Disable GPG checks.

6. Confirm the repo is enabled:

   ```bash
   yum repolist
   ```

7. Install packages using the local repository:

   ```bash
   yum --enablerepo=Local install zsh
   ```

## Patching/Updating a Package

### Patching Process

1. Communicate with the relevant team and ask for approval.
2. Inform the DB team to take a backup.
3. Turn off the application service and patch the package.
4. Use tools like ManageEngine, Spacewalk, or RedHat Satellite Server.

Before updating any package, ensure that you have explicit requirements to update. Don't simply run `yum update`.

### Kernel Update

When updating a kernel, the newer version doesn't replace the old one. Both versions are kept, and during boot, you can choose which kernel to use.

To check the kernel version:

```bash
rpm -qa | grep -i kernel
uname -r
```

To check for updates:

```bash
yum check-update kernel
```

## Rollback an Update / Yum Rollback

To revert the OS to a previous state:

```bash
yum history
yum history info <id-to-take-system-state-back-at>
yum history rollback <id-to-rollback-the-state-at>
```

To undo a rollback:

```bash
yum history undo <id-to-undo-rollback-to>
```

## 3. Source Code Installation / Tarball Installation

To install a package from its source code/tarball, download the package source code, extract and compile it, then install it.

### Steps for Source Code Installation

1. Download the tarball:

   ```bash
   wget https://dlcdn.apache.org/httpd/httpd-2.4.59.tar.bz2
   ```

2. Extract and navigate into the directory:

   ```bash
   tar jxvf httpd-2.4.59.tar.bz2
   cd httpd-2.4.59/
   ```

3. Run the `configure` script to check for dependencies and create the makefile:

   ```bash
   ./configure
   ```

4. Compile the package:

   ```bash
   make
   ```

5. Install the package:

   ```bash
   make install
   ```

### Customization

During the `./configure` step, you can customize the installation. For example:

```bash
./configure --enable=php --enable=java --disable=proxy
```

### Conclusion

This guide covers installation, upgrading, patching, and managing software packages on Linux using RPM, Yum, and source code/tarball compilation. For further details, refer to the respective documentation for each tool.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
