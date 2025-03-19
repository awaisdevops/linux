# MySQL Version Switcher

This guide provides instructions on how to install and switch between different versions of MySQL (5.6, 5.7, 8.0) on an Ubuntu-based system using a custom shell script.
  
## Install MySQL 8.0

To install MySQL 8.0, follow these steps:

```bash
sudo apt update
wget https://dev.mysql.com/get/mysql-apt-config_0.8.17-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.17-1_all.deb
sudo apt update
sudo apt install mysql-server
sudo systemctl status mysql
sudo mysql_secure_installation
sudo mysql -u root -p
sudo systemctl enable mysql
```

## Install MySQL 5.7

To install MySQL 5.7, follow these steps:

```bash
sudo apt update
sudo apt install wget -y
wget https://dev.mysql.com/get/mysql-apt-config_0.8.12-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.12-1_all.deb

# When prompted, choose Ubuntu Bionic and click Ok
# MySQL 8.0 is chosen by default. Choose the first option and click OK.
# Select MySQL 5.7 server and click OK.

sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys B7B3B788A8D3785C
sudo apt-get update
sudo apt-cache policy mysql-server
sudo apt install -f mysql-client=5.7* mysql-community-server=5.7* mysql-server=5.7*
sudo mysql_secure_installation
mysql -u root -p
```

## Install MySQL 5.6

To install MySQL 5.6, follow these steps:

```bash
mkdir /usr/local/mysql
cd /usr/local/mysql
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-server_5.6.44-1ubuntu14.04_amd64.deb-bundle.tar
```

## Create the MySQL Version Switch Script

To easily switch between different MySQL versions, create a custom shell script.

1. Open the script file for editing:

    ```bash
    sudo vim /bin/switch_mysql
    ```

2. Add the following script content:

    ```bash
    #!/bin/bash
    ## MySQL version switcher script

    current_version=$(mysqladmin --version|awk '{ print $5 }'|awk -F\, '{ print $1 }' | awk -F\. '{ print $1"."$2 }')
    current_version_8=$(mysqladmin --version|awk '{ print $3 }'|awk -F\, '{ print $1 }' | awk -F\. '{ print $1"."$2 }')

    read -p "Enter version of mysql to install (5.6 , 5.7 , 8.0) --> " selected_mysql_version

    if [[ $selected_mysql_version =~ 5.6 ]]; then
        if [[ $current_version == 5.6 ]]; then
            echo "MySQL version is already 5.6";
            exit;
        fi
        if [[ $current_version == 5.7 ]]; then
            sudo systemctl stop mysql;
            sudo mv /var/lib/mysql /var/lib/mysql_57;
            sudo apt-get -y remove --purge mysql-client mysql-common mysql-community-client mysql-community-server;
            if [[ -d "/var/lib/mysql_56" ]]; then
                sudo mv /var/lib/mysql_56 /var/lib/mysql;
            fi
            cd /usr/local/mysql;
            sudo dpkg -i mysql-common_5.6.44-1ubuntu14.04_amd64.deb;
            sudo dpkg -i libmysqlclient18_5.6.44-1ubuntu14.04_amd64.deb;
            sudo dpkg -i mysql-community-client_5.6.44-1ubuntu14.04_amd64.deb mysql-client_5.6.44-1ubuntu14.04_amd64.deb;
            sudo dpkg -i mysql-community-server_5.6.44-1ubuntu14.04_amd64.deb;
            sudo dpkg -i mysql-server_5.6.44-1ubuntu14.04_amd64.deb;
            sudo systemctl status mysql;
            sudo systemctl enable mysql;
        fi
        if [[ $current_version_8 == 8.0 ]]; then
            echo "Invalid downgrade path";
        fi
    elif [[ $selected_mysql_version =~ 5.7 ]]; then
        if [[ $current_version == 5.7 ]]; then
            echo "MySQL version is already 5.7";
            exit;
        fi
        if [[ $current_version == 5.6 ]]; then
            sudo systemctl stop mysql;
            sudo mv /var/lib/mysql /var/lib/mysql_56;
            sudo apt-get -y purge libmysqlclient18:amd64 mysql-client=5.6.44-1ubuntu14.04 mysql-community-client=5.6.44-1ubuntu14.04 mysql-server=5.6.44-1ubuntu14.04 mysql-community-server=5.6.44-1ubuntu14.04 mysql-common=5.6.44-1ubuntu14.04;
            if [[ -d "/var/lib/mysql_57" ]]; then
                sudo mv /var/lib/mysql_57 /var/lib/mysql;
            fi
            sudo apt-get -y install mysql-client=5.7* mysql-community-server=5.7*;
            sudo systemctl status mysql;
            sudo systemctl enable mysql;
        fi
        if [[ $current_version_8 == 8.0 ]]; then
            sudo systemctl stop mysql;
            sudo mv /var/lib/mysql /var/lib/mysql_8;
            sudo apt-get -y remove --purge mysql-client mysql-client-core-8.0 mysql-client-8.0 mysql-common mysql-server mysql-server-8.0 mysql-server-core-8.0;
            sudo rm -rf /etc/mysql;
            if [[ -d "/var/lib/mysql_57" ]]; then
                sudo mv /var/lib/mysql_57 /var/lib/mysql;
            fi
            sudo apt-get -y install mysql-client=5.7* mysql-community-server=5.7*;
            sudo systemctl status mysql;
            sudo systemctl enable mysql;
        fi
    elif [[ $selected_mysql_version =~ 8 ]]; then
        if [[ $current_version_8 == 8.0 ]]; then
            echo "MySQL version is already 8.0";
            exit;
        fi
        if [[ $current_version == 5.7 ]]; then
            sudo systemctl stop mysql;
            sudo mv /var/lib/mysql /var/lib/mysql_57;
            sudo apt-get -y remove --purge mysql-client mysql-common mysql-community-client mysql-community-server;
            sudo rm -rf /etc/mysql;
            if [[ -d "/var/lib/mysql_8" ]]; then
                sudo mv /var/lib/mysql_8 /var/lib/mysql;
            fi
            sudo apt-get -y install mysql-server mysql-client;
            sudo systemctl status mysql;
            sudo systemctl enable mysql;
        fi
        if [[ $current_version == 5.6 ]]; then
            echo "Invalid upgrade path";
        fi
    fi

    exit;
    ```

3. **Make the script executable**:

    ```bash
    chmod +x /bin/switch_mysql
    ```

## Switching MySQL Versions

Run the `switch_mysql` script to switch between the desired versions:

    ```bash
    switch_mysql
    ```

You will be prompted to choose a MySQL version (5.6, 5.7, or 8.0). Simply enter the desired version number.

## Conclusion

By following this guide, you can install MySQL 5.6, 5.7, and 8.0 on your system and easily switch between them using the `switch_mysql` script. This method allows for quick version switching and management for development or testing purposes.

If you encounter any issues or have questions, feel free to open an issue or contribute to the repository.

### Key Features:
- **Markdown Formatting**: The guide is written in markdown format, perfect for a GitHub `README.md` file.
- **Step-by-Step Instructions**: Each section is broken down clearly to guide users through installing different MySQL versions and setting up the version switcher script.
- **Custom Shell Script**: The `switch_mysql` script automates the process of switching between MySQL versions.
