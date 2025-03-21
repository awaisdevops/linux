# Elasticsearch Installation and Version Switcher

This guide provides instructions for installing multiple versions of Elasticsearch on a Linux system and switching between them using a script.

## Steps to Install Elasticsearch Versions

### Step 1: Download Elasticsearch Versions

Navigate to `/usr/local` and download the required Elasticsearch versions:

```bash
cd /usr/local

# Download multiple versions of Elasticsearch
wget https://download.elastic.co/elasticsearch/elasticsearch/elasticsearch-1.7.5.deb
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.4.0.deb
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.0.deb
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.0.deb
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.4.deb
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.6.0-amd64.deb
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.9.0-amd64.deb
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.4.3-amd64.deb
```

### Step 2: Rename Elasticsearch Packages

Rename the downloaded packages to a consistent naming scheme:

```bash
mv elasticsearch-7.6.0-amd64.deb elasticsearch-7.6.0.deb
mv elasticsearch-7.9.0-amd64.deb elasticsearch-7.9.0.deb
mv elasticsearch-8.4.3-amd64.deb elasticsearch-8.4.3.deb
```

### Step 3: Create the Switcher Script

Create the script that allows you to switch between Elasticsearch versions:

```bash
vim /bin/switch_elasticsearch
```

Add the following script content:

```bash
#!/bin/bash
read -p "Select elasticsearch version (1.7.5 , 5.4.0, 5.6.0, 6.2.0, 6.2.4, 7.6.0, 7.9.0, 8.4.3) --> " elastic_version

if [[ $elastic_version == 1.7.5 || $elastic_version == 5.4.0 || $elastic_version == 5.6.0 || $elastic_version == 6.2.4 || $elastic_version == 6.2.0 ]]; then
    sudo apt-get purge elasticsearch;
    sudo rm -rf /var/lib/elasticsearch;
    sudo update-java-alternatives --jre-headless --set java-1.8.0-openjdk-amd64
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk;
    sudo sed -i "s/java-11-openjdk/java-8-openjdk/g" /etc/bash.bashrc;
    source /etc/bash.bashrc;
    sudo dpkg -i /usr/local/elasticsearch-$elastic_version.deb;
    sleep 2;

    if [[ $elastic_version == 1.7.5 ]]; then
        echo "discovery.zen.ping.multicast.enabled: false" >> /etc/elasticsearch/elasticsearch.yml;
    fi

    sudo systemctl start elasticsearch;
    sudo systemctl enable elasticsearch;
fi

if [[ $elastic_version == 7.6.0 || $elastic_version == 7.9.0 || $elastic_version == 8.4.3 ]]; then
    sudo apt-get purge elasticsearch;
    sudo rm -rf /var/lib/elasticsearch;

    sudo update-java-alternatives --jre-headless --set java-1.11.0-openjdk-amd64
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk;
    sudo sed -i "s/java-8-openjdk/java-11-openjdk/g" /etc/bash.bashrc;
    source /etc/bash.bashrc;

    sudo dpkg -i /usr/local/elasticsearch-$elastic_version.deb;
    sudo systemctl start elasticsearch;
    sudo systemctl enable elasticsearch;
fi

echo "Waiting for Elasticsearch to start...";
sleep 10;
curl -X GET http://localhost:9200;
```

### Step 4: Make the Script Executable

Make the script executable by running:

```bash
chmod +x /bin/switch_elasticsearch
```

### Step 5: Switch Elasticsearch Version

To switch the Elasticsearch version, simply run:

```bash
switch_elasticsearch
```

Follow the on-screen prompts to select the Elasticsearch version you wish to use.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
