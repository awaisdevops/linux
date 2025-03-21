# Running Multiple Java Versions at Once

This guide provides instructions on how to install multiple Java versions (8, 11, 17, and 21) on a Linux system and switch between them using a simple script.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Step 1: Install Java Versions](#step-1-install-java-versions)
- [Step 2: Create Java Switch Script](#step-2-create-java-switch-script)
- [Step 3: Make the Script Executable](#step-3-make-the-script-executable)
- [Step 4: Switch Java Versions](#step-4-switch-java-versions)
- [License](#license)

## Prerequisites

Make sure you have `sudo` privileges on your system to install packages and modify files.

## Step 1: Install Java Versions

Install the Java versions you want to use on your Linux system. Run the following command to install Java 8, 11, 17, and 21:

```bash
sudo apt install openjdk-8-jdk openjdk-11-jdk openjdk-17-jdk openjdk-21-jdk
```

This will install the required Java Development Kit (JDK) versions.

## Step 2: Create Java Switch Script

Create a script to easily switch between Java versions. To do this, follow these steps:

1. Open the terminal and run the following command to create the switch script:

```bash
sudo vim /bin/switch_java
```

2. Add the following script to the file:

```bash
#!/bin/bash

read -p "Java version (8, 11, 17, or 21): " java_ver;

if [[ $java_ver == 8 ]]; then
    sudo update-java-alternatives --jre-headless --set java-1.8.0-openjdk-amd64
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk;
    sudo sed -i "s/java-11-openjdk/java-8-openjdk/g" /etc/bash.bashrc;

elif [[ $java_ver == 11 ]]; then
    sudo update-java-alternatives --jre-headless --set java-1.11.0-openjdk-amd64
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk;
    sudo sed -i "s/java-8-openjdk/java-11-openjdk/g" /etc/bash.bashrc;

elif [[ $java_ver == 17 ]]; then
    sudo update-java-alternatives --jre-headless --set java-1.17.0-openjdk-amd64
    export JAVA_HOME=/usr/lib/jvm/java-17-openjdk;
    sudo sed -i "s/java-11-openjdk/java-17-openjdk/g" /etc/bash.bashrc;

elif [[ $java_ver == 21 ]]; then
    sudo update-java-alternatives --jre-headless --set java-1.21.0-openjdk-amd64
    export JAVA_HOME=/usr/lib/jvm/java-21-openjdk;
    sudo sed -i "s/java-17-openjdk/java-21-openjdk/g" /etc/bash.bashrc;

else
    echo "Invalid Java version selected. Please choose 8, 11, 17, 21, or 22."
    exit 1;
fi

source /etc/bash.bashrc;
exit
```

3. Save and exit the editor.

## Step 3: Make the Script Executable

Next, make the `switch_java` script executable. Run the following command:

```bash
sudo chmod +x /bin/switch_java
```

## Step 4: Switch Java Versions

You can now switch between the installed Java versions by running the following command:

```bash
switch_java
```

Follow the prompts (8, 11, 17, and 21) to choose the Java version you want to use. The script will update the system and set the selected Java version.

## License

This guide is provided under the MIT License. See the LICENSE file for more details.
