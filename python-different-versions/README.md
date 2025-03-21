# Managing Multiple Versions of Python on Ubuntu 20.04

This guide demonstrates how to install and manage multiple versions of Python on Ubuntu 20.04 or newer using the Deadsnakes PPA. It provides step-by-step instructions to ensure you can install Python versions in parallel without interfering with the system Python installation.

## Steps to Install Multiple Versions of Python

### Step 1: Add the Deadsnakes PPA

The Deadsnakes PPA (Personal Package Archive) is a repository that provides more recent versions of Python for Ubuntu. To add the repository, run:

```bash
sudo add-apt-repository ppa:deadsnakes/ppa
```

You'll be prompted with information about the repository. Press Enter to continue.

### Step 2: Update Your Package List

After adding the PPA, update your package list:

```bash
sudo apt update
```

### Step 3: Install the Desired Python Versions

Now you can install the version(s) of Python you need. For example, to install Python 3.9 and Python 3.8, run:

```bash
sudo apt install python3.9 python3.8
```

You can install any other versions of Python available through Deadsnakes, such as Python 3.10 or Python 3.11.

### Step 4: Configure Python Versions Using `update-alternatives`

After installing multiple Python versions, use the `update-alternatives` command to configure which version should be used by default. For example:

```bash
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 1
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 2
```

This sets Python 3.9 as the default and Python 3.8 as an alternative.

### Step 5: Switch Between Python Versions

You can switch between Python versions using `update-alternatives`. To select which version to use, run:

```bash
sudo update-alternatives --config python3
```

You'll be prompted to choose which version to use.

### Step 6: Verify the Python Version

After switching versions, verify the active Python version:

```bash
python3 --version
```

This will display the currently active version of Python.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
