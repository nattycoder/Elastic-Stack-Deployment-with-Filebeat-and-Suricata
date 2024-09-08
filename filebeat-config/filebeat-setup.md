# Filebeat Installation and Configuration Guide

This guide walks you through the process of installing and configuring Filebeat, a lightweight data shipper for log files.

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Installing Filebeat](#installing-filebeat)
4. [Configuring Filebeat](#configuring-filebeat)
5. [Enabling Filebeat Modules](#enabling-filebeat-modules)
6. [Setting Up Ingest Pipelines](#setting-up-ingest-pipelines)
7. [Loading Index Template](#loading-index-template)
8. [Loading Kibana Dashboards](#loading-kibana-dashboards)
9. [Starting and Enabling Filebeat](#starting-and-enabling-filebeat)
10. [Verifying Elasticsearch Data](#verifying-elasticsearch-data)

## Introduction

Filebeat is part of the Elastic Stack and is used to collect and ship log files. Other Beats include:
- Metricbeat: collects system and service metrics
- Packetbeat: analyzes network data
- Winlogbeat: collects Windows event logs
- Auditbeat: collects Linux audit framework data and monitors file integrity
- Heartbeat: monitors service availability

## Prerequisites

- Elasticsearch and Logstash should be installed and running on your system.
- Ensure you have sudo or root access to your Ubuntu system.

## Installing Filebeat

Install Filebeat using APT:

```bash
sudo apt install filebeat
```

## Configuring Filebeat

1. Open the Filebeat configuration file:

```bash
sudo nano /etc/filebeat/filebeat.yml
```

2. Disable the Elasticsearch output by commenting out these lines:

```yaml
#output.elasticsearch:
  # Array of hosts to connect to.
  #hosts: ["localhost:9200"]
```

3. Enable the Logstash output:

```yaml
output.logstash:
  # The Logstash hosts
  hosts: ["localhost:5044"]
```

## Enabling Filebeat Modules

In this guide, we'll use the Suricata module as an example:

1. Enable the Suricata module:

```bash
sudo filebeat modules enable suricata
```

2. Verify that the module is enabled:

```bash
sudo filebeat modules list
```

## Setting Up Ingest Pipelines

Load the ingest pipeline for the Suricata module:

```bash
sudo filebeat setup --pipelines --modules suricata
```

## Loading Index Template

Load the index template into Elasticsearch:

```bash
sudo filebeat setup --index-management -E output.logstash.enabled=false -E 'output.elasticsearch.hosts=["localhost:9200"]'
```

## Loading Kibana Dashboards

Load the sample Kibana dashboards:

```bash
sudo filebeat setup -E output.logstash.enabled=false -E output.elasticsearch.hosts=['localhost:9200'] -E setup.kibana.host=localhost:5601
```

## Starting and Enabling Filebeat

Start and enable Filebeat to run on system boot:

```bash
sudo systemctl start filebeat
sudo systemctl enable filebeat
```

## Verifying Elasticsearch Data

To verify that Elasticsearch is receiving data from Filebeat:

```bash
curl -XGET 'http://localhost:9200/filebeat-*/_search?pretty'
```

You should see output similar to:

```json
{
  "took" : 3,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 0,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  }
}
```

If `"value": 0` in the `"total"` field, Elasticsearch is not receiving any logs, and you should review your setup for errors.

## Conclusion

You have successfully installed and configured Filebeat to collect and ship log data to Logstash and Elasticsearch. For more advanced configuration options and usage, refer to the [official Filebeat documentation](https://www.elastic.co/guide/en/beats/filebeat/current/index.html).

Remember to keep your Elastic Stack components updated for optimal performance and security.
