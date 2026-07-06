# 🏗️ Splunk Architecture

## Overview

Splunk is a powerful SIEM platform that collects, indexes, and analyzes machine data from any source. Understanding its architecture is fundamental to deploying and managing it effectively in a SOC environment.

---

## Core Components

| Component | Role |
|-----------|------|
| **Forwarder** | Collects and sends data to the indexer |
| **Indexer** | Receives, parses, and stores data |
| **Search Head** | Provides UI for searching and visualizing data |

---

## Types of Forwarders

| Type | Description | Use Case |
|------|-------------|----------|
| **Universal Forwarder (UF)** | Lightweight agent, minimal resource usage, forwards raw data | Most common — used in this lab |
| **Heavy Forwarder (HF)** | Full Splunk instance, can parse/filter/route data before forwarding | Enterprise environments with complex routing |

> ✅ This lab uses **Universal Forwarder (UF)** on both Windows 11 and Ubuntu Desktop.

---

## Lab Architecture

```
┌─────────────────┐     ┌─────────────────┐
│   Windows 11    │     │ Ubuntu Desktop  │
│  (UF Agent)     │     │  (UF Agent)     │
│                 │     │                 │
│ - Security logs │     │ - Syslog        │
│ - System logs   │     │ - Auth logs     │
│ - App logs      │     │ - Snort alerts  │
└────────┬────────┘     └────────┬────────┘
         │                       │
         └──────────┬────────────┘
                    │ Port 9997 (TCP)
                    ▼
         ┌──────────────────────┐
         │   Splunk Enterprise  │
         │   (Ubuntu Server)    │
         │                      │
         │ - Indexer            │
         │ - Search Head        │
         │ - Web UI (Port 8000) │
         └──────────────────────┘
```

---

## Data Flow

1. **Collection** — Universal Forwarder monitors log files and event logs on endpoints
2. **Forwarding** — Logs are sent to Splunk Enterprise over TCP port 9997
3. **Indexing** — Splunk parses and stores data in indexes
4. **Searching** — SPL queries are used to search, filter, and analyze indexed data
5. **Visualization** — Results displayed as dashboards, charts, and alerts

---

## Key Concepts

| Concept | Description |
|---------|-------------|
| **Index** | Storage container for data (e.g., `windows_logs`, `linux_logs`) |
| **Sourcetype** | Defines the format/type of data (e.g., `WinEventLog:Security`) |
| **Source** | The origin file or input of the data |
| **Host** | The machine that generated the data |
| **Event** | A single log entry in Splunk |

---

## Important Ports

| Port | Purpose |
|------|---------|
| 8000 | Splunk Web UI |
| 9997 | Forwarder to Indexer communication |
| 8089 | Splunk management port |
| 514 | Syslog input |

---

## References

- [Splunk Architecture Documentation](https://docs.splunk.com/Documentation/Splunk/latest/Deploy/Distributedoverview)
- [Splunk Forwarder Types](https://docs.splunk.com/Documentation/Forwarder/latest/Forwarder/Typesofforwarders)
