# Lab 02 – pfSense Integration with Wazuh

**Author:** Muhammad Faisal Farooq
**Position:** SOC Intern – ITSOLERA Pvt. Ltd.
**Date:** *(Add date)*
**Source:** ITSOLERA Internship Tasks Booklet

---

## Objective

The objective of this lab is to integrate pfSense with Wazuh to achieve centralized network visibility, monitoring, and alerting.
This includes configuring pfSense to forward logs to Wazuh and enabling additional network security services such as Suricata, pfBlockerNG, and Squid with ClamAV.
The final goal is to detect, analyze, and document security events generated from the firewall, IDS/IPS, and proxy.

---

## Lab Environment

* pfSense Firewall – 2 vCPU, 4 GB RAM
* Wazuh Manager (OVA) – 4 vCPU, 8 GB RAM
* Kali Linux (Test Client) – 2 vCPU, 2 GB RAM
* VMware Workstation – Virtualization Platform
* LAN Network: 192.168.10.0/24
* pfSense LAN Gateway: 192.168.10.1
* Wazuh Manager IP: 192.168.10.5
* Kali Test Machine IP: 192.168.10.10

---

## Network Diagram

Internet → pfSense (Suricata, pfBlockerNG, Squid + ClamAV) → LAN Network → [Kali Test Client] → [Wazuh Manager]

---

## Step-by-Step Setup

### 1. Deploy pfSense

* Download pfSense ISO from official website.
* Create a VM with two network adapters (WAN and LAN).
* Install pfSense and assign static LAN IP 192.168.10.1/24.
* Access pfSense via browser at `https://192.168.10.1` and log in as `admin / pfsense`.
* Verify both WAN and LAN interfaces are active.

### 2. Deploy Wazuh OVA

* Import the official Wazuh OVA in VMware.
* Start the VM and check the assigned IP address.
* Access the Wazuh dashboard at `https://<WAZUH_IP>`.
* Confirm that Wazuh Manager, Indexer, and Dashboard services are active.

### 3. Configure Remote Syslog (pfSense → Wazuh)

* In pfSense, go to `Status > System Logs > Settings > Remote Logging Options`.
* Add the Wazuh IP address and select UDP port 514.
* Enable remote logging for Firewall, System, Suricata, and Proxy logs.
* Save and apply the settings.
* On Wazuh, confirm receipt of logs using:

  ```
  sudo tcpdump -n -i any port 514
  ```

  Logs should appear as syslog traffic from pfSense.

### 4. Install and Configure Suricata

* Open pfSense and navigate to `System > Package Manager > Available Packages`.
* Install Suricata and enable it on the LAN interface.
* Enable EVE JSON output for structured logging.
* Select rule sources such as EmergingThreats Open.
* Start Suricata service and verify that alerts are generated.

### 5. Install and Configure pfBlockerNG

* Install pfBlockerNG-devel from Package Manager.
* Enable GeoIP blocking and select threat intelligence feeds such as Abuse.ch or FireHOL.
* Apply the configuration and verify blocked IPs in the Firewall logs.

### 6. Install Squid and ClamAV

* Install Squid Proxy Server and enable Transparent Proxy mode for LAN.
* Install and enable ClamAV for malware scanning within Squid.
* Enable logging for both access and antivirus events.
* Optionally configure HTTPS inspection for deeper scanning (optional for lab).

### 7. Configure Wazuh Decoders and Rules

* Create a local decoder file:

  ```
  /var/ossec/etc/decoders/local_decoder_pfsense.xml
  ```

  Example content:

  ```xml
  <decoders>
    <decoder name="pfsense-firewall">
      <program_name>filterlog</program_name>
    </decoder>
    <decoder name="suricata-alert">
      <program_name>suricata</program_name>
    </decoder>
    <decoder name="squid-proxy">
      <program_name>squid</program_name>
    </decoder>
  </decoders>
  ```

* Add custom rules:

  ```
  /var/ossec/etc/rules/local_rules.xml
  ```

  Example:

  ```xml
  <group name="pfSense,">
    <rule id="100100" level="8">
      <decoded_as>pfsense-firewall</decoded_as>
      <description>pfSense: Firewall Block Event Detected</description>
    </rule>
    <rule id="100200" level="10">
      <decoded_as>suricata-alert</decoded_as>
      <description>Suricata Alert Detected on pfSense</description>
    </rule>
    <rule id="100300" level="12">
      <decoded_as>squid-proxy</decoded_as>
      <description>Squid Proxy (ClamAV) – Malware Blocked</description>
    </rule>
  </group>
  ```

* Restart Wazuh Manager:

  ```
  sudo systemctl restart wazuh-manager
  ```

### 8. Test the Setup (EICAR Simulation)

* From Kali, execute:

  ```
  wget http://www.eicar.org/download/eicar.com
  ```
* Expected behavior:

  * SquidClamAV blocks the file.
  * Suricata generates a detection alert.
  * Wazuh shows a correlated alert event in the dashboard.

---

## Verification Results

| Test Description             | Expected Output                    | Result |
| ---------------------------- | ---------------------------------- | ------ |
| pfSense logs appear in Wazuh | Firewall logs visible in dashboard | Passed |
| Suricata alerts received     | IDS alerts parsed correctly        | Passed |
| pfBlockerNG blocks IP        | Log entry forwarded to Wazuh       | Passed |
| EICAR download blocked       | Alert generated in Wazuh           | Passed |

---

## Observations

* Syslog forwarding was initially blocked due to firewall rule restrictions. Allowing UDP 514 resolved the issue.
* Suricata on LAN produced cleaner, relevant alerts compared to WAN deployment.
* pfBlockerNG added external threat intelligence capability.
* ClamAV successfully intercepted and logged EICAR test download attempts.
* Custom Wazuh decoders allowed structured and categorized alerting.

---

## Challenges

* Required manual decoder adjustments for pfSense log format.
* SSL inspection in Squid required manual CA import into the browser.
* Initial syslog traffic not reaching Wazuh until proper firewall rule added.

---

## Lessons Learned

* Centralizing pfSense logs into Wazuh significantly enhances SOC visibility.
* IDS, proxy, and firewall integration provide layered detection capability.
* Proper decoder and rule customization are essential for accurate alerting.
* SSL inspection introduces complexity but increases coverage.

---

## SOC Relevance

* pfSense provides perimeter visibility.
* Suricata contributes intrusion detection at network level.
* Squid and ClamAV offer web-layer malware control.
* Wazuh unifies all logs for correlation, detection, and incident response.

This lab demonstrates the foundation of a real SOC environment, combining multiple network and host data sources for correlation.

---

## Recommendations

* Integrate VirusTotal for automatic hash lookups in Wazuh.
* Automate IOC updates between Wazuh and pfBlockerNG.
* Build dashboards in Kibana for network-level visibility.
* Implement automated blocking playbooks for high-confidence events.

---

## Conclusion

The integration between pfSense and Wazuh was successfully completed.
Firewall, IDS, and proxy logs were centralized into Wazuh and correlated effectively.
All test scenarios, including the EICAR malware simulation, were successfully detected and logged.
This lab demonstrates how network-level visibility and SIEM correlation form a complete SOC defense layer.

---

## **End of Lab 02 – pfSense Integration with Wazuh**
