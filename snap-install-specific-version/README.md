# Install Specific Version of LXD Using Snap

This guide provides instructions on how to install a specific version of any package using Snap channels.

Here we're considering lxd package for demonstration.

## LXD Snap Channels

Snap offers various channels for LXD, each providing different versions. Below is a list of available channels and their respective versions:

### Available Channels

- **latest/stable**:  
  Version: 5.14-7072c7b  
  Released: 2023-06-01  
  Size: 178MB  

- **latest/candidate**:  
  Version: 5.14-7072c7b  
  Released: 2023-05-29  
  Size: 178MB  

- **latest/beta**:  
  No release available at the moment.

- **latest/edge**:  
  Version: git-6bdccb9  
  Released: 2023-06-20  
  Size: 181MB  

- **5.14/stable**:  
  No release available at the moment.

- **5.14/candidate**:  
  Version: 5.14-7072c7b  
  Released: 2023-05-31  
  Size: 178MB  

- **5.14/beta**:  
  No release available at the moment.

- **5.14/edge**:  
  No release available at the moment.

- **5.13/stable**:  
  Version: 5.13-8e2d7eb  
  Released: 2023-05-31  
  Size: 174MB  

- **5.13/candidate**:  
  No release available at the moment.

- **5.13/beta**:  
  No release available at the moment.

- **5.13/edge**:  
  No release available at the moment.

---

## Install LXD from a Specific Channel

To install a specific version of LXD, you need to choose the appropriate Snap channel. For instance, if you want to install the **5.14/candidate** version, you can run the following command:

```bash
snap install lxd --channel=5.14/candidate
```

This command will install the **5.14/candidate** version of LXD. You can replace `5.14/candidate` with any other channel version you prefer.

---

## Verify Installed Version

To check the version of LXD installed and ensure the correct version has been selected, use the following command:

```bash
snap info lxd
```

This will show the version information and available channels.

---

## Conclusion

You can install specific versions of LXD by specifying the desired channel when using the `snap install` command. Choose the appropriate channel based on your needs and ensure you install the correct version of LXD.


This `README.md` provides a structured guide to installing a specific version of LXD using Snap, detailing the available channels and how to install a version of your choice.
