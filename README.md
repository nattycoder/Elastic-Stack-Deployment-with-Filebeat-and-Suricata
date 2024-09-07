# SIEM Solution Deployment Project

This project demonstrates the deployment of a comprehensive Security Information and Event Management (SIEM) solution using Security Onion, pfSense, and various open-source security tools.

## Project Overview

This SIEM solution integrates the following components:
- Security Onion (latest version) as the core SIEM platform
- pfSense (latest version) as the firewall
- Suricata as the Network Intrusion Detection System (NIDS)
- ELK Stack (Elasticsearch, Logstash, Kibana) for log management and visualization
- Filebeat for log shipping

## Architecture

```
[Internet] <-> [pfSense + Suricata] <-> [LAN] <-> [Security Onion]
                     |                               |
                     |                               |
                  Filebeat --------------------------> Logstash
                                                      |
                                                      v
                                                 Elasticsearch
                                                      ^
                                                      |
                                                    Kibana
```

## Key Features

1. pfSense firewall configuration with LAN and WAN interfaces
2. Suricata NIDS deployment on pfSense
3. Custom detection rules for both LAN and WAN interfaces
4. Filebeat installation and configuration on pfSense
5. Custom Logstash pipeline on Security Onion
6. Custom Kibana ingest pipeline for processing Suricata alerts
7. Kibana dashboard for visualizing Suricata alerts

## Repository Structure

- `/pfsense-config/`: pfSense configuration files
- `/suricata-config/`: Custom Suricata rules
- `/filebeat-config/`: Filebeat configuration files
- `/logstash-config/`: Custom Logstash pipeline configuration
- `/elasticsearch-config/`: Custom Elasticsearch ingest pipeline configuration
- `/kibana-config/`: Kibana dashboard export
- `/docs/`: Additional documentation and setup guides

## Setup and Configuration

Please refer to the following guides for detailed setup instructions:

1. [pfSense and Suricata Setup](docs/pfsense-suricata-setup.md)
2. [Filebeat Configuration](docs/filebeat-setup.md)
3. [Security Onion Configuration](docs/security-onion-setup.md)
4. [Logstash Pipeline Setup](docs/logstash-pipeline-setup.md)
5. [Kibana_and Elasticsearch_Configuration](docs/kibana-elasticsearch-setup.md)

## Usage

After setting up the SIEM solution, you can:

1. Monitor network traffic and alerts through the Suricata interface on pfSense
2. View and analyze logs in the Security Onion interface
3. Use the custom Kibana dashboard to visualize Suricata alerts and other relevant data

## Contributing

Contributions to this project are welcome. Please fork the repository and submit a pull request with your improvements.
