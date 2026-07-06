# Splunk-SIEM-Lab

# 📊 Splunk SIEM Home Lab

![Splunk](https://img.shields.io/badge/Splunk-SIEM-black?style=for-the-badge&logo=splunk)
![Platform](https://img.shields.io/badge/Platform-Windows%2011%20%7C%20Ubuntu-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)
![Blue Team](https://img.shields.io/badge/Team-Blue%20Team-blue?style=for-the-badge)

A hands-on Splunk SIEM home lab covering Splunk architecture, Universal Forwarder deployment on Windows 11 & Ubuntu, and SPL (Search Processing Language) for log analysis and threat hunting — designed to simulate real SOC workflows.

---

## 🖥️ Lab Architecture

| Component | Role | OS |
|-----------|------|----|
| Splunk Enterprise | SIEM / Indexer / Search Head | Ubuntu |
| Universal Forwarder 1 | Log collection & forwarding | Windows 11 |
| Universal Forwarder 2 | Log collection & forwarding | Ubuntu Desktop |

---

## ✅ Implemented Modules

### 1. 🏗️ Splunk Architecture
- Understood core Splunk components: Forwarder, Indexer, Search Head
- Configured data inputs, indexes, and sourcetypes
- 📄 [Architecture Documentation](docs/architecture.md)

---

### 2. 📡 Universal Forwarder (UF)
- Deployed Universal Forwarder on **Windows 11** and **Ubuntu Desktop**
- Configured forwarders to send logs to Splunk Enterprise
- Monitored system logs, security events, and application logs
- 📄 [Universal Forwarder Documentation](docs/universal-forwarder.md)

---

### 3. 🔍 SPL (Search Processing Language)
- Learned and applied SPL for log searching and analysis
- Used commands for filtering, statistics, and visualization
- Built searches to detect suspicious activity
- 📄 [SPL Documentation](docs/spl.md)

---

## 📁 Repository Structure

```
Splunk-SIEM-Lab/
│
├── README.md
├── configs/
│   └── universal-forwarder.conf    # UF inputs & outputs config
└── docs/
    ├── architecture.md             # Splunk architecture overview
    ├── universal-forwarder.md      # UF setup guide
    └── spl.md                      # SPL commands & examples
```

---

## 🚀 Getting Started

### Prerequisites
- Splunk Enterprise (free trial at splunk.com)
- Splunk Universal Forwarder installed on endpoints
- Network connectivity between forwarder and Splunk server

### Quick Setup
1. Install Splunk Enterprise on Ubuntu
2. Install Universal Forwarder on Windows 11 & Ubuntu
3. Configure `inputs.conf` and `outputs.conf` on each forwarder
4. Start receiving logs in Splunk Dashboard

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| Splunk Enterprise | SIEM / Log Indexer / Search Head |
| Splunk Universal Forwarder | Log collection from endpoints |
| SPL | Search & analysis language |
| Windows 11 | Monitored endpoint |
| Ubuntu Desktop | Monitored endpoint |

---

## 🔍 SPL Queries Highlights

```spl
# Search all events
index=* | head 100

# Count events by source
index=* | stats count by source

# Detect failed logins
index=* EventCode=4625 | stats count by Account_Name

# Top source IPs
index=* | top limit=10 src_ip

# Events over time
index=* | timechart count by sourcetype
```

---

## 📸 Screenshots
<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/b5131a5a-bf40-46c9-8895-1aa05a5cde0f" />
<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/4c342dc0-3b13-444e-a3c5-4d009b96a1a4" />
<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/b9720a8c-28cd-4938-be8a-b5657f5489b9" />
<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/8f07276c-b86b-496a-bb01-060de1889b32" />
<img width="1920" height="1080" alt="5" src="https://github.com/user-attachments/assets/8a46f93f-842a-49e2-8527-c7b65de8f069" />


---

## 📚 References

- [Splunk Official Documentation](https://docs.splunk.com/)
- [SPL Reference Manual](https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/WhatsInThisManual)
- [Splunk Universal Forwarder Guide](https://docs.splunk.com/Documentation/Forwarder/latest/Forwarder/Abouttheuniversalforwarder)
- [MITRE ATT&CK Framework](https://attack.mitre.org/)

---

## 👤 Sahil Kute

**Your Name**
- 🔗 [LinkedIn](https://linkedin.com/in/sahil-kute)
- 🐙 [GitHub](https://github.com/Sahilkute)

---

## 📜 License

This project is for educational purposes only. Use responsibly.

---

> ⭐ If you found this helpful, consider giving it a star!
