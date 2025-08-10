# File Integrity Monitoring (FIM) – Setup Guide

## 📌 Objective
Simulate a real-world SOC scenario to detect unauthorized file modifications using Wazuh’s File Integrity Monitoring (FIM) feature. The lab replicates centralized log collection and analysis in a secure environment.

---

## 🛠️ Lab Environment
| Component          | Details |
|--------------------|---------|
| **SIEM Server**    | Wazuh Manager (OVA on VMware Workstation) |
| **Endpoint(s)**    | Kali Linux OVA (monitored host) |
| **Host Machine**   | Local PC (Windows 11, 16GB RAM, Intel i7) |
| **Network**        | Host-Only Adapter for isolated communication |


## ⚙️ Step-by-Step Setup

### 1️⃣ Deploy Wazuh Manager
1. Download Wazuh OVA from the [official Wazuh downloads page](https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html).
2. Import OVA into VMware Workstation.
3. Verify Wazuh Manager is running:

   ```bash
   sudo systemctl status wazuh-manager
   ```


### 2️⃣ Configure the Endpoint (Kali Linux)

1. Goto Wazuh Dashboard:
login Credentials:
username: admin
password: admin

- Click on deploy new agent.
- Select appropriate platform and input configurations like server ip and host name.

   ```bash
   wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.11.1-1_amd64.deb && sudo WAZUH_MANAGER='192.168.10.7' dpkg -i ./wazuh-agent_4.11.1-1_amd64.deb
   ```
2. Edit the agent configuration:

   ```bash
   sudo nano /var/ossec/etc/ossec.conf
   ```

   * Set `<address>` to Wazuh Manager IP.
3. Enable and start the agent:

   ```bash
   sudo systemctl enable wazuh-agent
   sudo systemctl start wazuh-agent
   ```

---

### 3️⃣ Enable File Integrity Monitoring

1. On the Wazuh Manager, edit `ossec.conf`:

   ```bash
   sudo nano /var/ossec/etc/ossec.conf
   ```
2. Add monitored directories:

   ```xml
   <directories check_all="yes" report_changes="yes" realtime="yes">/root</directories>
   ```
3. Restart the manager:

   ```bash
   sudo systemctl restart wazuh-manager
   ```


### 4️⃣ Testing & Verification

1. On Kali, create or modify a file in `/root`:

   ```bash
   sudo nano /root/samplefile.txt
   touch kali.txt
   ```
and add other some sample files, edit them and delete some.

2. Check the Wazuh Dashboard → Security Events → File Integrity Monitoring.


## 📊 Outcome

- Configured centralized Wazuh environment.
- Enabled File Integrity Monitoring on a remote endpoint.
- Successfully detected unauthorized file changes in a SOC-like setup.



## 🚀 Next Steps

- Integrate with Wazuh rules for custom alerting.
- Test on multiple endpoints (Windows & Linux).
- Combine with host-based intrusion detection for broader coverage.


**Author:** Muhammad Faisal Farooq
**Date:** 2025-08-10

```

---

```
