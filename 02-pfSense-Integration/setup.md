# Lab 02 – pfSense Integration with Wazuh
<p style="text-align:center;">
<img width="981" height="463" alt="image" src="https://github.com/user-attachments/assets/746ac74d-87cb-432e-9018-87b789cb44e0" />
</p>

## Introduction

This lab covers the integration of a **pfSense firewall** with the **Wazuh SIEM** to enhance network security and monitoring.
pfSense is configured with **pfBlockerNG** to block specific websites and countries, while **Wazuh** collects and analyzes firewall logs in real time for visibility and alerting.
This setup simulates a practical security environment for traffic control and incident detection.

---

## What is Wazuh?

**Wazuh** is an open-source security platform that helps detect threats, monitor system integrity, and ensure compliance with regulations.
It acts as a **Security Information and Event Management (SIEM)** tool by collecting, parsing, and analyzing data from endpoints and cloud environments.

It consists of the following main components:

* **Wazuh Manager** – Collects and analyzes data from agents.
* **Wazuh Agent** – Installed on monitored endpoints.
* **Elastic Stack** – Used to visualize and analyze data (includes Elasticsearch, Logstash, and Kibana).

---

## What is pfSense?

**pfSense** is an open-source firewall and router platform based on **FreeBSD**.
It provides enterprise-grade features such as stateful packet filtering, VPN support, intrusion detection/prevention, and traffic shaping — all managed through an easy-to-use web interface.

pfSense can be deployed on both physical hardware and virtual machines, making it ideal for home, business, and lab environments.
Its modular design allows extensions through packages like **pfBlockerNG** for advanced content filtering and **GeoIP-based blocking**.

---

## Installing Wazuh OVA in VMware
<p style="text-align:center">
<img width="666" height="450" alt="image" src="https://github.com/user-attachments/assets/e5159744-3e46-431d-8f9b-c6dd45bbbb05" />
</p>

To begin, download and install the official **Wazuh OVA (Open Virtual Appliance)** file in VMware Workstation.

### Steps:

1. Downloaded the Wazuh OVA from the official Wazuh website.
2. Opened **VMware Workstation** and selected **“Open a Virtual Machine”**.
3. Imported the OVA and adjusted settings (RAM, CPU as needed).
4. Started the VM and waited for Wazuh to boot up.

<img width="670" height="572" alt="image" src="https://github.com/user-attachments/assets/eaca83cd-10f4-4179-898f-422fc8c902c2" />


---

## Starting Up the Wazuh Server

<img width="675" height="387" alt="image" src="https://github.com/user-attachments/assets/594fefcd-068e-4c05-8b8a-859c278ea769" />


Once the virtual machine was up:

* Logged in with default credentials:

  * **Username:** `wazuh-user`
  * **Password:** `wazuh`
* Verified that all services were running (`wazuh-manager`, `dashboard`, `indexer`, etc.).
* Accessed the Wazuh dashboard through the browser using the IP assigned to the VM.
* The IP can be found using commands:

  ```
  ip a
  ip address
  ifconfig
  ```
<img width="1124" height="321" alt="image" src="https://github.com/user-attachments/assets/9f6e9df8-d87b-4446-b69d-d12b405364a1" />

---

## Accessing the Wazuh Dashboard

<img width="713" height="397" alt="image" src="https://github.com/user-attachments/assets/39c08ad6-225b-48c8-866d-4bb63c47c4de" />


* Opened a browser and entered:

  ```
  https://<Wazuh_VM_IP>
  ```
* Logged into the Wazuh dashboard.
* Navigated through dashboards, rules, alerts, and logs in the **Wazuh App**.

---

## Configuring pfSense

<img width="779" height="450" alt="image" src="https://github.com/user-attachments/assets/966d81ef-a056-4cee-a38a-b80e20ce0b3c" />


1. Download the latest NetGate installer from [https://www.pfsense.org/download/](https://www.pfsense.org/download/).
2. Import it into VMware and install the pfSense Firewall.
3. Carefully assign virtual network interfaces:

   * **WAN adapter:** connected to the Internet
   * **LAN adapter:** connected to internal hosts (Kali endpoint and Wazuh SIEM)
4. Access pfSense through its LAN IP address in a browser and log in using default credentials:

   * **Username:** `admin`
   * **Password:** `pfsense`
5. Verify that both WAN and LAN are correctly configured and operational.

<img width="929" height="431" alt="image" src="https://github.com/user-attachments/assets/64c5c915-7dfa-4a18-847e-042700ca955d" />
<img width="871" height="565" alt="image" src="https://github.com/user-attachments/assets/1f8c97f7-3ac1-4bc8-9c07-496415c62a47" />


---

## Installing pfBlockerNG

* Navigate to **System > Package Manager > Available Packages**.
* Search for **pfBlockerNG** and install it.
* After installation, configure pfBlockerNG according to network requirements — allowing or blocking specific countries, websites, or IP feeds.

<img width="1050" height="667" alt="image" src="https://github.com/user-attachments/assets/ae535bcf-fd65-45a7-b425-dfdb52b2a132" />

---

## Firewall Configuration

<img width="1056" height="210" alt="image" src="https://github.com/user-attachments/assets/67ae882c-aa5d-488d-82fa-22ae0ef9e233" />

* Assign **WAN** as the outgoing traffic interface.
* Assign **LAN** as the incoming traffic interface.
* Enable **Remote Logging Options** to forward logs from pfSense to the Wazuh SIEM for analysis and monitoring.

<img width="1052" height="138" alt="image" src="https://github.com/user-attachments/assets/e95d38ff-6be4-45f5-a029-0e56031be6a9" />
<img width="1000" height="632" alt="image" src="https://github.com/user-attachments/assets/dd3b9b16-de6d-4663-926c-631d6c265d9f" />

---

## Configuring Wazuh Dashboard

<img width="261" height="337" alt="image" src="https://github.com/user-attachments/assets/e25c111d-4716-46a2-8cf5-2993aefb4c3b" />

1. Go to the **Wazuh Dashboard** and click on **Server Management**.
2. Open **Settings** and edit configurations to integrate pfSense with Wazuh.

<img width="1056" height="171" alt="image" src="https://github.com/user-attachments/assets/577ff670-ec68-4674-90de-046597430192" />
<img width="732" height="226" alt="image" src="https://github.com/user-attachments/assets/9ab948f5-064b-48f5-8c12-f15e78c76f92" />

---

## Adding a Decoder for pfSense

<img width="414" height="519" alt="image" src="https://github.com/user-attachments/assets/b8ca4b36-bf38-40d1-9d31-4efc8b23ba41" />
<img width="973" height="140" alt="image" src="https://github.com/user-attachments/assets/ebd8f1ff-7131-4a63-b316-bfcd28995220" />


**What is a Decoder?**
In Wazuh, a *decoder* is a set of rules that define how raw log messages are interpreted and broken into structured fields.
Decoders use patterns (regular expressions) to identify elements like IP addresses, ports, actions, or protocols so that Wazuh can apply rules, generate alerts, and store meaningful data.

A custom decoder can be created to parse pfSense firewall logs into readable fields for better monitoring and analysis.

---

## Writing Wazuh Rules

<img width="452" height="591" alt="image" src="https://github.com/user-attachments/assets/1ded05fd-6573-4123-b212-33b55713ff0a" />

**What are Rules?**
In Wazuh, *rules* are conditions that evaluate decoded log data to detect specific events or behaviors.
Rules define which patterns, thresholds, or field values should trigger an alert and can assign severity levels, categories, and messages.

For example:

* A rule may alert when pfSense logs show blocked traffic from a restricted country.
* Another rule may detect multiple failed login attempts or repeated access to blacklisted domains.

Rules work together with decoders to transform raw logs into actionable security intelligence.

---

## Monitoring pfSense Logs in Wazuh

Once the integration, decoders, and rules are configured, pfSense logs start appearing in Wazuh in real time.
Firewall events such as blocked IPs, traffic anomalies, and access violations are displayed in the Wazuh dashboard under **Security Events**.

---

## Testing the Integration

<img width="995" height="543" alt="image" src="https://github.com/user-attachments/assets/dd18d6ac-7b92-4b79-a858-b201ac23d50f" />

To verify functionality:

* Attempted an SSH login from an unauthorized external user to the internal network.
* The attempt was blocked by pfSense, logged by the firewall, and detected by Wazuh as an alert.
* Confirmed that the event appeared in the Wazuh dashboard with proper rule matching and severity classification.

---

## Conclusion

The integration of **pfSense Firewall** with **Wazuh SIEM** provided a unified view of network traffic and security events, enabling real-time monitoring, correlation, and alerting.
By centralizing firewall logs into Wazuh, visibility into malicious attempts, policy violations, and potential intrusions was significantly improved.

This task strengthened the defensive posture of the environment and demonstrated the critical role of combining perimeter security with SIEM analytics.
Through this implementation, I gained hands-on experience in:

* Network defense
* Log analysis
* Rule and decoder management
* SOC-driven threat detection workflows

This lab represents an important step toward building expertise as a Blue Team cybersecurity professional.

---

**End of Lab 02 – pfSense Integration with Wazuh**

---
