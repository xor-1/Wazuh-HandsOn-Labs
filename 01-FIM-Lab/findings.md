```markdown
# Findings – File Integrity Monitoring (FIM) Lab

## 🔍 Overview
This document summarizes the key observations, insights, and learnings from the File Integrity Monitoring (FIM) lab performed using Wazuh SIEM in a simulated SOC environment.

---

## 1️⃣ Key Observations
- **Centralized Logging Works as Intended**  
  The Wazuh Manager successfully collected logs from the Kali Linux endpoint over a host-only network without exposing the monitored system directly.
  
- **FIM Detected All Modifications**  
  Any addition, deletion, or modification to files within `/etc` and `/var/www` triggered alerts in the Wazuh dashboard.

- **Timestamp & User Context Provided**  
  Each alert contained file path, event type, timestamp, and the user responsible — essential details for SOC triage.

---

## 2️⃣ Testing Results
| Test Action                        | Expected Result                          | Actual Result | Status |
|------------------------------------|-------------------------------------------|--------------|--------|
| Create new file in `/root`         | Alert triggered in Wazuh dashboard        | ✅ Alerted   | Pass   |
| Modify existing `/root`            | Alert triggered with "modified" status    | ✅ Alerted   | Pass   |
| Delete monitored file in `/root`   | Alert triggered with "deleted" status     | ✅ Alerted   | Pass   |

---

## 3️⃣ Challenges Faced
- **Network Configuration** – Ensuring the Wazuh Manager and endpoint communicated over an isolated network without internet leakage.  
- **Agent Registration** – Initial failure due to wrong manager IP in `ossec.conf`. Fixed by reassigning static IPs and re-registering the agent.  

---

## 4️⃣ Lessons Learned
- Always verify **agent-to-manager connectivity** before troubleshooting deeper issues.  
- File Integrity Monitoring requires careful **selection of directories** to avoid excessive false positives.  
- Maintaining **separation between monitoring system and monitored endpoint** improves realism and security.  

---

## 5️⃣ SOC Relevance
- This lab mirrors a real-world SOC scenario where analysts monitor endpoint file changes from a secure centralized platform.  
- Skills gained:  
  - Deploying & configuring SIEM  
  - Writing and applying monitoring rules  
  - Interpreting and validating security events  
  - Incident triage workflow  

---

**Author:** Muhammad Faisal Farooq  
**Date:** 2025-08-10
```

---
