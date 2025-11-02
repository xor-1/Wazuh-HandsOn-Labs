# Lab 02 – pfSense Firewall Integration with Wazuh

## Objective
Integrate pfSense with Wazuh SIEM by forwarding firewall and IDS logs to the Wazuh Manager, creating decoders and rules to parse them, and verifying visibility in the Security Events dashboard.

## Lab Environment
- **pfSense VM** (firewall and IDS/IPS)
- **Wazuh OVA** (SIEM)
- **Kali Linux or Windows endpoint** (for generating traffic)
- **VMware Workstation** with dual network adapters (WAN/LAN)
- **Network Mode:** Bridged or Host-only depending on your setup

## Step-by-Step Setup

1. **Deploy pfSense VM**
   - Download the ISO from [pfSense.org](https://www.pfsense.org/download/).
   - Configure WAN (DHCP) and LAN (Static IP, e.g., 192.168.10.1/24).
   - Access web UI: `https://192.168.10.1` (default credentials `admin/pfsense`).

2. **Install Wazuh OVA**
   - Import official Wazuh OVA and start services.
   - Confirm access via `https://<WAZUH_IP>`.

3. **Enable Remote Syslog in pfSense**
   - Go to **Status → System Logs → Settings → Remote Logging Options**.
   - Add **Wazuh Manager IP** and **Port 514 (UDP)**.
   - Check “Everything” or specifically `firewall`, `system`, `suricata` if installed.

4. **Test Connectivity**
   - On Wazuh VM: `sudo tcpdump -i any port 514` (to confirm logs arriving).
   - If logs appear, proceed.

5. **Create Decoder in Wazuh**
   - Path: `/var/ossec/etc/decoders/pfsense_decoders.xml`
   - Example snippet:
     ```xml
     <decoder name="pfsense-firewall">
       <program_name>filterlog</program_name>
     </decoder>
     ```

6. **Create Rule in Wazuh**
   - Path: `/var/ossec/etc/rules/local_rules.xml`
     ```xml
     <rule id="100100" level="5">
       <decoded_as>pfsense-firewall</decoded_as>
       <description>pfSense firewall log detected</description>
     </rule>
     ```
   - Restart manager: `sudo systemctl restart wazuh-manager`

7. **Verification**
   - Generate a blocked connection (e.g., SSH or ICMP from WAN).
   - Confirm alert in Wazuh Security Events → pfSense logs.

## Testing & Verification
| Test | Expected Result | Verified |
|------|------------------|-----------|
| Syslog connection established | Logs visible in `/var/ossec/logs/ossec.log` | ✅ |
| pfSense block event visible | Alert generated in Wazuh | ✅ |

## Outcome
pfSense firewall logs are successfully centralized in Wazuh, enabling detection and correlation with other assets.

## Next Steps
- Add Suricata alerts
- Configure dashboards
- Tune rules for production

