# 🔍 SPL (Search Processing Language)

## Overview

SPL (Search Processing Language) is Splunk's powerful query language used to search, filter, analyze, and visualize machine data. It is a core skill for any SOC analyst using Splunk.

---

## SPL Syntax Structure

```spl
index=<index_name> <filters> | command1 | command2 | command3
```

---

## Essential SPL Commands

| Command | Description | Example |
|---------|-------------|---------|
| `search` | Filter events | `index=* error` |
| `stats` | Calculate statistics | `stats count by host` |
| `table` | Display specific fields | `table _time, host, source` |
| `fields` | Include/exclude fields | `fields + src_ip, dest_ip` |
| `where` | Filter with expressions | `where count > 10` |
| `eval` | Create/modify fields | `eval risk=if(count>100,"high","low")` |
| `rex` | Extract fields with regex | `rex field=_raw "user=(?<username>\w+)"` |
| `timechart` | Time-based chart | `timechart count by sourcetype` |
| `top` | Top values of a field | `top limit=10 src_ip` |
| `rare` | Least common values | `rare limit=5 user` |
| `dedup` | Remove duplicates | `dedup host` |
| `sort` | Sort results | `sort -count` |
| `head` | First N results | `head 20` |
| `tail` | Last N results | `tail 20` |
| `rename` | Rename a field | `rename src_ip as Source_IP` |

---

## SOC Relevant SPL Queries

### 🔐 Authentication & Login Analysis

```spl
# Failed Windows logins (Event ID 4625)
index=windows_logs EventCode=4625
| stats count by Account_Name, src_ip
| sort -count

# Successful logins after failures (possible brute force success)
index=windows_logs EventCode=4624
| stats count by Account_Name, src_ip

# Failed SSH logins on Linux
index=linux_logs sourcetype=linux_secure "Failed password"
| rex field=_raw "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by src_ip, user
| sort -count

# Top failed login usernames
index=* (EventCode=4625 OR "Failed password")
| stats count by user
| sort -count
| head 10
```

---

### 🌐 Network Analysis

```spl
# Top source IPs
index=* | top limit=10 src_ip

# Top destination IPs
index=* | top limit=10 dest_ip

# Events by sourcetype over time
index=* | timechart count by sourcetype

# Traffic volume by host
index=* | stats count by host | sort -count
```

---

### 🦠 Threat Hunting Queries

```spl
# Detect port scanning (many connections from same IP)
index=* | stats dc(dest_port) as port_count by src_ip
| where port_count > 20
| sort -port_count

# Detect large data transfers (possible exfiltration)
index=* | stats sum(bytes) as total_bytes by src_ip
| where total_bytes > 10000000
| sort -total_bytes

# Detect PowerShell execution on Windows
index=windows_logs EventCode=4688 Process_Name="*powershell*"
| table _time, host, Account_Name, Process_Name, Process_Command_Line

# Detect new user creation
index=windows_logs EventCode=4720
| table _time, host, Account_Name, Subject_Account_Name
```

---

### 📊 Dashboard & Reporting Queries

```spl
# Event count over time
index=* | timechart span=1h count

# Top 10 events by host
index=* | stats count by host | sort -count | head 10

# Security events summary
index=windows_logs
| stats count by EventCode
| sort -count

# Log volume by index
index=* | stats count by index | sort -count
```

---

## Time Modifiers

```spl
# Last 15 minutes
index=* earliest=-15m latest=now

# Last 24 hours
index=* earliest=-24h latest=now

# Specific date range
index=* earliest="01/01/2024:00:00:00" latest="01/31/2024:23:59:59"

# Last 7 days
index=* earliest=-7d latest=now
```

---

## Field Extraction with rex

```spl
# Extract IP address from raw log
index=linux_logs
| rex field=_raw "from (?<src_ip>\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"
| stats count by src_ip

# Extract username from auth log
index=linux_logs
| rex field=_raw "user=(?<username>[a-zA-Z0-9_]+)"
| stats count by username
```

---

## References

- [SPL Reference Manual](https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/WhatsInThisManual)
- [Splunk Search Tutorial](https://docs.splunk.com/Documentation/Splunk/latest/SearchTutorial/WelcometotheSearchTutorial)
- [Splunk Security Essentials](https://splunkbase.splunk.com/app/3435)
