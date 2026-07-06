# 📡 Universal Forwarder (UF) Setup Guide

## Overview

The Splunk Universal Forwarder is a lightweight agent installed on endpoints to collect and forward logs to Splunk Enterprise. It uses minimal resources and is the most common forwarder type used in SOC environments.

---

## Lab Setup

| Agent | OS | Logs Collected |
|-------|----|----------------|
| UF Agent 1 | Windows 11 | Security, System, Application, PowerShell logs |
| UF Agent 2 | Ubuntu Desktop | Syslog, Auth logs, Snort alerts |

---

## Installation

### On Ubuntu Desktop

```bash
# Download Universal Forwarder (check splunk.com for latest version)
wget -O splunkforwarder.deb "https://download.splunk.com/products/universalforwarder/releases/latest/linux/splunkforwarder-latest-linux-amd64.deb"

# Install
sudo dpkg -i splunkforwarder.deb

# Start Splunk UF and enable boot start
sudo /opt/splunkforwarder/bin/splunk start --accept-license
sudo /opt/splunkforwarder/bin/splunk enable boot-start

# Add Splunk Enterprise as receiving indexer
sudo /opt/splunkforwarder/bin/splunk add forward-server YOUR_SPLUNK_IP:9997

# Verify connection
sudo /opt/splunkforwarder/bin/splunk list forward-server
```

### On Windows 11

```powershell
# Download Splunk UF MSI installer from splunk.com
# Run installer as Administrator

# OR install via command line:
msiexec.exe /i splunkforwarder.msi RECEIVING_INDEXER="YOUR_SPLUNK_IP:9997" AGREETOLICENSE=Yes /quiet

# Start the forwarder service
& "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" start

# Add indexer
& "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" add forward-server YOUR_SPLUNK_IP:9997
```

---

## Configuration Files

### outputs.conf (both agents)
Tells the forwarder where to send data.
```ini
[tcpout]
defaultGroup = splunk_indexer

[tcpout:splunk_indexer]
server = YOUR_SPLUNK_IP:9997
```

### inputs.conf (Ubuntu)
Tells the forwarder what to collect.
```ini
[monitor:///var/log/syslog]
index = linux_logs
sourcetype = syslog

[monitor:///var/log/auth.log]
index = linux_logs
sourcetype = linux_secure
```

### inputs.conf (Windows 11)
```ini
[WinEventLog://Security]
index = windows_logs
sourcetype = WinEventLog:Security
disabled = false
```

---

## Verify Logs in Splunk

```spl
# Check Ubuntu logs
index=linux_logs | head 20

# Check Windows logs
index=windows_logs | head 20

# Check which hosts are sending data
index=* | stats count by host
```

---

## Config File

📄 [universal-forwarder.conf](../configs/universal-forwarder.conf)

---

## References

- [Splunk UF Installation Guide](https://docs.splunk.com/Documentation/Forwarder/latest/Forwarder/Installtheuniversalforwardersoftware)
- [Splunk inputs.conf Reference](https://docs.splunk.com/Documentation/Splunk/latest/Admin/Inputsconf)
