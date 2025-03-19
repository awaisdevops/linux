# Linux Compression and Archive Tools

This guide provides an overview of Linux backup tools, backup types, and compression utilities. It also includes various command examples for remote backups using `rsync` and `scp`, along with information on popular compression and archiving tools.

## Backup Tools in Linux

### Common Backup Tools

- **gzip**
- **bzip2**
- **xz**
- **tar**
- **zip**
- **cp**

For remote backup, the tools `scp` and `rsync` are commonly used. They utilize the SSH protocol on port 22.

---

## Backup Types

### Rsync
Rsync is a powerful tool used for remote incremental backups. After the first full backup, rsync only transfers newly added files or files that have changed (based on modification time). It compares the modification times of files and synchronizes the source and destination.

#### Rsync Synopsis

```bash
rsync FLAGS SOURCE DESTINATION
```

#### Common Rsync Flags

- `-v` – Verbose output.
- `-r` – Recursively copy directories (but doesn’t preserve timestamps or permissions).
- `-a` – Archive mode, preserving symbolic links, file permissions, user/group ownerships, and timestamps.
- `-z` – Compress files during transfer to reduce network usage.
- `-h` – Human-readable file sizes.
- `-P` – Show progress during the transfer.

#### Rsync Command Examples

```bash
rsync -avzh <source> <destination>
# Copy/Sync Directory Locally

rsync -avzh /root/rpmpkgs root@server-address:/root/
# Copy a Directory from Local to Remote Server

rsync -avzh root@server-address:/root/rpmpkgs /tmp/myrpms
# Copy a Directory from Remote to Local Server

rsync -avzhe ssh root@server-address:/root/anaconda-ks.cfg /tmp
# Copy a File from a Remote Server to a Local Server with SSH

rsync -avzhe ssh backup.tar.gz root@server-address:/backups/
# Copy a File from a Local Server to a Remote Server with SSH

rsync -avz --include='*.txt' /path/to/source/ user@remote:/path/to/destination/
# Include Files with Particular Extension with Rsync

rsync -avz --exclude='*.ext' /path/to/source/ user@remote:/path/to/destination/
# Exclude Files with Particular Extension with Rsync

rsync --remove-source-files -zvh backup.tar.gz root@server-address:/tmp/backups/
# Automatically Delete Source Files After Transfer

```

### SCP

`scp` is a secure copy command for transferring files between hosts using SSH.

#### SCP Command Examples

```bash
scp scp-cheatsheet.pdf tecmint@server-address:/home/scp-data/
# Copy File From Local Host to Remote Server

scp tecmint@server-address:/home/ssh-cheatsheet.pdf /home/scp-data/
# Copy File From Remote Host to Local Host

scp -v tecmint@server-address:/home/ssh-cheatsheet.pdf tecmint@server-address:/home/server-data/
# Copy File From Remote Host to Another Host

scp -p scp-cheatsheet.pdf tecmint@server-address:/home/tecmint/
# Copy Files with Original Creation Date and Time (use -p)

scp -Cp messages.log mrarianto@server-address:/opt/test/
# SCP Compression While Copying Files (use -C)

scp -P 2231 root@server-address:/home/rtftp-client1_044056.tar.gz /var/www/html/
# SCP with a Different Port (use -P)

scp -r documents mrarianto@server-address:/opt/test/
# SCP – Copy Files and Directories Recursively (use -r)
```

---

## Commercial Backup Tools

Some commonly used commercial backup tools include:

- **Veritas NetBackup**
- **Windows NT Backup**
- **Dell Networker**
- **EMC Networker**
- **VEEAM**

### Open Source Backup Tools for Linux

Some open-source backup tools available for Linux are:

- **AMANDA**
- **ZAMANDA**
- **BACULA**
- **BACKUPC**

---

## Stat Command

You can use the `stat` command to check the access time, modification time, change time (which occurs when ownership or permissions are modified), inode number, and other metadata for files.

```bash
stat file5
```

---

## Compression and Archiving Tools

### Compression Tools

#### 1. Gzip

To compress files using `gzip`:

```bash
gzip file1
#compression
gunzip file1.gz
#de-compression
```

To read a gzip-compressed file:

```bash
zcat my_file.gz
```

#### 2. Bzip2

To compress files using `bzip2`:

```bash
bzip2 file2
#compression
bunzip2 file2.bz2
#decompression
```

To read a bzip2-compressed file:

```bash
bzcat file1.bz2
```

#### 3. XZ

To compress files using `xz`:

```bash
xz file3
#compression
unxz file3.xz
#de-compression
```

To read an xz-compressed file:

```bash
xzcat file3.xz
```

---

### Archive + Compression Tools

#### 1. Zip

To create a zip archive:

```bash
zip my_file.zip file1 file2 dir1/
#compression+archival
unzip my_file.zip
#de-compression+archival
```

To unzip into a different directory:

```bash
unzip -d /tmp/new-dir/ haproxy.zip
```

To list files in a zip archive without extracting:

```bash
unzip -l all.zip
```

#### 2. Tar

The `tar` command is used for archiving files. It does not perform compression on its own, but can be combined with other compression tools like `gzip` or `bzip2`.

```bash
tar -cvf wwe.tar *
# Create a tar archive
tar -xvf wwe.tar
# Extract a tar archive
```

#### Tar + Compression Tools

##### Tar + Gzip

```bash
tar -zcvf cpp.tar.gz *
# Archive and compress using gzip
tar -zxvf cpp.tar.gz
# Extract a tar.gz archive
```

##### Tar + Bzip2

```bash
tar -jcvf minion.tar.bz *
# Archive and compress using bzip2
tar -jxvf minion.tar.bz
# Extract a tar.bz2 archive
```

To exclude specific files during the archiving process:

```bash
tar -zcvf backup.tar.gz --exclude='zi8v8B52,error_log,dev_crm' /home/crmrekadv/public_html/
```

To list the contents of a tar archive:

```bash
tar -tvf wwe.tar
```

---

## Redirection

Linux supports several types of redirection:

1. `>` (overwrite) – Redirect output to a file, overwriting the file.
2. `>>` (append) – Append output to the end of a file.
3. `|` (pipe) – Use the output of one command as the input for another command.

---

This guide provides essential tools and commands for backups, compression, and archiving in Linux, along with practical examples for each.
