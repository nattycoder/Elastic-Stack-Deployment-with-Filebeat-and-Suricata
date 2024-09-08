# Suricata Installation and Configuration Guide

Suricata is a high-performance Network IDS, IPS, and Network Security Monitoring engine. This guide will walk you through the installation and basic configuration of Suricata on Ubuntu.

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
3. [Configuration](#configuration)
4. [Verification and Management](#verification-and-management)

## Prerequisites

Ensure you have sudo or root access to your Ubuntu system.

## Installation

### Update System Packages

First, update and upgrade your system packages:

```bash
sudo apt update -y
sudo apt upgrade -y
```

### Install Dependencies

Install necessary dependencies:

```bash
sudo apt install gnupg2 software-properties-common curl wget git unzip -y
```

### Add Suricata Repository

Add the official Suricata repository:

```bash
sudo add-apt-repository ppa:oisf/suricata-stable --yes
```

### Update Repository Cache

Update the repository cache:

```bash
sudo apt update
```

### Verify Suricata Package

Check the available Suricata package:

```bash
sudo apt-cache policy suricata
```

Expected output:
```
suricata:
  Installed: (none)
  Candidate: 1:6.0.4-3
  Version table:
     1:6.0.4-3 500
        500 http://us.archive.ubuntu.com/ubuntu jammy/universe amd64 Packages
```

### Install Suricata

Install Suricata and jq (a lightweight command-line JSON processor):

```bash
sudo apt install suricata jq
```

### Verify Installation

Confirm the installation by checking Suricata's build information:

```bash
suricata --build-info
```

## Configuration

### Edit Suricata Configuration File

Open the Suricata configuration file:

```bash
sudo nano /etc/suricata/suricata.yaml
```

Modify the following lines:

```yaml
HOME_NET: "[1.2.3.4/24]" # Specify your network 
EXTERNAL_NET: "!$HOME_NET"

af-packet:
  - interface: eth0 # Replace with your network interface

sip:
  enabled: no
```

Save and exit the file (Ctrl+X, then Y, then Enter).

### Update Suricata

Update Suricata's rulesets:

```bash
sudo suricata-update
```

### Verify Configuration

Check if the configuration file is valid:

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml -v
```

## Verification and Management

### Enable and Start Suricata Service

Enable and start the Suricata service:

```bash
sudo systemctl enable --now suricata
```

### Check Suricata Service Status

Verify that Suricata is running:

```bash
sudo systemctl status suricata
```

Expected output:
```
suricata.service - LSB: Next Generation IDS/IPS
     Loaded: loaded (/etc/init.d/suricata; generated)
     Active: active (running)
```

### List Available Run Modes

View various run modes:

```bash
suricata --list-runmodes
```

### Monitor Suricata Logs

View the Suricata log file in real-time:

```bash
sudo tail -f /var/log/suricata/fast.log
```

## Conclusion

You have successfully installed and configured Suricata on your Ubuntu system. For more advanced configuration and usage, refer to the [official Suricata documentation](https://suricata.readthedocs.io/).

Remember to regularly update Suricata and its rulesets to ensure optimal security coverage.
