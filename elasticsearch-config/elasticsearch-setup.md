# Elasticsearch Installation and Configuration Guide

This guide will walk you through the process of installing and configuring Elasticsearch on Ubuntu.

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Adding Elasticsearch Repository](#adding-elasticsearch-repository)
3. [Installing Elasticsearch](#installing-elasticsearch)
4. [Configuring Elasticsearch](#configuring-elasticsearch)
5. [Starting and Enabling Elasticsearch](#starting-and-enabling-elasticsearch)
6. [Verifying Installation](#verifying-installation)

## Prerequisites

Ensure you have sudo or root access to your Ubuntu system.

## Adding Elasticsearch Repository

1. Import the Elasticsearch public GPG key:

```bash
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg
```

This command uses `curl` to download the GPG key and `gpg` to convert it into a format that APT can use.

2. Add the Elastic source list:

```bash
echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```

This adds the Elasticsearch repository to your system's package sources.

3. Update the package lists:

```bash
sudo apt update
```

## Installing Elasticsearch

Install Elasticsearch using APT:

```bash
sudo apt install elasticsearch
```

## Configuring Elasticsearch

1. Open the Elasticsearch configuration file:

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

2. Find the line that specifies `network.host`, uncomment it, and set its value to `localhost`:

```yaml
# ---------------------------------- Network -----------------------------------
#
# Set the bind address to a specific IP (IPv4 or IPv6):
#
network.host: localhost
```

This configuration restricts access to Elasticsearch, allowing only local connections.

Save and close the file (Ctrl+X, then Y, then Enter).

## Starting and Enabling Elasticsearch

1. Start the Elasticsearch service:

```bash
sudo systemctl start elasticsearch
```

2. Enable Elasticsearch to start on boot:

```bash
sudo systemctl enable elasticsearch
```

## Verifying Installation

Test if Elasticsearch is running by sending an HTTP request:

```bash
curl -X GET "localhost:9200"
```

You should see a response with basic information about your local Elasticsearch node.

## Conclusion

You have successfully installed and configured Elasticsearch on your Ubuntu system. For more advanced configuration and usage, refer to the [official Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html).

Remember to secure your Elasticsearch instance properly, especially if you plan to expose it to external networks.
