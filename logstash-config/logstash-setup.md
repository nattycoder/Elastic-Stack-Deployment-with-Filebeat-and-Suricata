# Logstash Installation and Configuration Guide

This guide walks you through the process of installing and configuring Logstash, a data processing pipeline that ingests, transforms, and ships data.

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Installing Logstash](#installing-logstash)
4. [Configuring Logstash](#configuring-logstash)
   - [Input Configuration](#input-configuration)
   - [Output Configuration](#output-configuration)
5. [Testing the Configuration](#testing-the-configuration)
6. [Starting and Enabling Logstash](#starting-and-enabling-logstash)

## Introduction

Logstash is often used to process data before sending it to Elasticsearch. It allows you to collect data from various sources, transform it into a common format, and export it to different destinations.

## Prerequisites

- Elasticsearch should be installed and running on your system.
- Ensure you have sudo or root access to your Ubuntu system.

## Installing Logstash

Install Logstash using APT:

```bash
sudo apt install logstash
```

## Configuring Logstash

Logstash configuration files are located in the `/etc/logstash/conf.d` directory. A Logstash pipeline has two required elements (input and output) and one optional element (filter).

### Input Configuration

1. Create a configuration file for Beats input:

```bash
sudo nano /etc/logstash/conf.d/01-beats-input.conf
```

2. Add the following input configuration:

```
input {
  beats {
    port => 5044
  }
}
```

This configuration sets up a Beats input that listens on TCP port 5044.

### Output Configuration

1. Create a configuration file for Elasticsearch output:

```bash
sudo nano /etc/logstash/conf.d/10-elasticsearch-output.conf
```

2. Add the following output configuration:

```
output {
  if [@metadata][pipeline] {
    elasticsearch {
      hosts => ["localhost:9200"]
      manage_template => false
      index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
      pipeline => "%{[@metadata][pipeline]}"
    }
  } else {
    elasticsearch {
      hosts => ["localhost:9200"]
      manage_template => false
      index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
    }
  }
}
```

This configuration tells Logstash to store the Beats data in Elasticsearch, which is running at `localhost:9200`, in an index named after the Beat used.

## Testing the Configuration

Test your Logstash configuration with this command:

```bash
sudo -u logstash /usr/share/logstash/bin/logstash --path.settings /etc/logstash -t
```

If there are no syntax errors, you should see output similar to:

```
Config Validation Result: OK. Exiting Logstash
```

Note: You may receive warnings from OpenJDK, but these can typically be ignored if the validation result is OK.

## Starting and Enabling Logstash

If your configuration test is successful, start and enable Logstash:

```bash
sudo systemctl start logstash
sudo systemctl enable logstash
```

This will start Logstash and configure it to start automatically on system boot.

## Conclusion

You have successfully installed and configured Logstash to receive data from Beats and forward it to Elasticsearch. For more advanced configuration options and usage, refer to the [official Logstash documentation](https://www.elastic.co/guide/en/logstash/current/index.html).

Remember to keep your Elastic Stack components updated for optimal performance and security.
