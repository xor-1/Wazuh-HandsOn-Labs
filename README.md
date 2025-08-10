# 🛡️ Wazuh Security Operations Labs

[![Role Badge](https://img.shields.io/badge/Role-SOC%20Analyst%20(Training)-blue)](#)
[![SIEM Badge](https://img.shields.io/badge/SIEM-Wazuh-orange)](#)
[![Lab Type](https://img.shields.io/badge/Type-Hands--On%20Labs-brightgreen)](#)

---

## 📌 About This Repository
This repository contains a **series of hands-on security operations labs using Wazuh**.  
Each lab is designed to simulate **real-world SOC workflows**, covering **endpoint monitoring, log analysis, firewall integration, custom rule creation, network threat detection, and more**.  

All labs are built in **virtualized environments** (VMware) with **centralized SIEM architecture**, reflecting how security monitoring is done in professional environments.

---

## 🎯 Goals
- Develop **practical SOC skills** using Wazuh as a SIEM platform.
- Simulate **realistic security monitoring and incident response workflows**.
- Gain expertise in **log analysis, rule tuning, and alert correlation**.
- Build a **portfolio of cybersecurity projects** for career readiness.

---

## 🏗️ Planned & Completed Labs
- ✅ **File Integrity Monitoring (FIM)** — Agent-based monitoring & alerting  
- 🔄 **pfSense Firewall Integration** — Centralized firewall log analysis  
- 🔄 **Network Traffic Monitoring** — Detect suspicious traffic patterns  
- 🔄 **Custom SIEM Rule Development** — Create and fine-tune detection logic  
- 🔄 **Threat Hunting in Wazuh** — Search and investigate historical data  
- 🔄 **Incident Reporting Workflow** — From alert to analyst report  

---

## 📂 Repository Structure
```plaintext
Wazuh-Labs/
│
├── 01-FIM-Lab/
│   ├── setup-guide.md
│   ├── screenshots/
│   └── findings.md
│
├── 02-pfSense-Integration/
│   ├── setup-guide.md
│   ├── rules/
│   └── analysis.md
│
├── 03-Network-Monitoring/
│   └── ...
│
└── README.md
```

🚀 Skills You’ll See Across Labs
- Centralized SIEM deployment
- Endpoint agent installation & configuration
- File Integrity Monitoring (FIM)
- Firewall log ingestion & analysis
- Network activity monitoring
- Custom detection rule creation
- SOC alert triage & incident handling
- Threat hunting techniques
- and much more...

⚙️ Technology Stack (But not limited to...)
- Wazuh (SIEM & security monitoring)
- VMware Workstation
- Kali Linux
- pfSense Firewall
- Linux CLI tools
- Sysmon (Windows labs planned)
  
📊 Typical Lab Architecture
[ Analyst Machine ]
        |
        v
[ Wazuh Manager (SIEM) ]
        |
        v
[ Multiple Monitored Endpoints ]

- Analyst Machine: For reviewing alerts and reports.
- Wazuh Manager: Centralized log collection & rule processing.
- Endpoints: Windows, Linux, and network devices generating security events.

💡 About Me
I’m Muhammad Faisal Farooq, a cybersecurity student and aspiring SOC Analyst and Red Team Enthusiast.
This repository documents my continuous learning journey in SIEM operations, monitoring, and threat detection — preparing for a global career in cybersecurity.

Some of my accomplishments are:
SOC Analyst Intern @ ITSOLERA | Red Teamer @ AUCSS Red Team | ISC2 CC | Google Cybersecurity | ISO /IEC 27001:2022 | AI in Red Teaming | CTFs @ AIRXORIUM

Connect with me on LinkedIn:
https://www.linkedin.com/in/muhammadfaisalfarooq/
