# 🛡️ Lab 2: Network Intrusion Detection (Suricata on Kali Agent)
*Goal:* Detect network-based attacks (like Nmap scans) by integrating Suricata IDS with Wazuh.

---

## 🔹 Part 1: Agent Configuration (Kali Linux)
Install Suricata and configure the Wazuh Agent to read its logs.

### Step A: Install & Configure Suricata
1. Install Suricata on the Ubuntu endpoint. We tested this process with version 6.0.8 and it can take some time:
```bash
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt-get update
sudo apt-get install suricata -y
```

2. Download and extract the Emerging Threats Suricata ruleset:
```bash
cd /tmp/ && curl -LO https://rules.emergingthreats.net/open/suricata-6.0.8/emerging.rules.tar.gz
sudo tar -xvzf emerging.rules.tar.gz && sudo mv rules/*.rules /etc/suricata/rules/
sudo chmod 640 /etc/suricata/rules/*.rules
```

3. Modify Suricata settings in the /etc/suricata/suricata.yaml file and set the following variables:
```bash
sudo nano /etc/suricata/suricata.yaml
```

```JavaScript
HOME_NET: "<UBUNTU_IP>"
EXTERNAL_NET: "any"

default-rule-path: /etc/suricata/rules
rule-files:
- "*.rules"

# Global stats configuration
stats:
enabled: Yes

# Linux high speed capture support
af-packet:
  - interface: eth0
```

4. Restart the Suricata service:
```bash
sudo systemctl restart suricata
```

### Step B: Configure Wazuh Agent
1.  *Edit Agent Config:*
    bash
    sudo nano /var/ossec/etc/ossec.conf
    
2.  *Add Log Collection Block:*
    * Add this block inside the <ossec_config> section to read Suricata's EVE JSON log:
    xml
    <localfile>
      <log_format>json</log_format>
      <location>/var/log/suricata/eve.json</location>
    </localfile>
    
3.  *Restart Wazuh Agent:*
    bash
    sudo systemctl restart wazuh-agent
    

---

## 🔹 Part 2: Verification & Results
Simulate a network scan to trigger the alert.

1.  *Trigger the Event (Attack):*
    * You need to scan the Kali machine from another device (e.g., your Windows VM or Host Machine).
    * If you don't have a second machine, you can try scanning localhost from Kali itself, though external scans are better.
    * *Command (from attacker):*
    bash
    nmap -A -sS -p- <KALI_IP_ADDRESS>
    
2.  *Check Wazuh Dashboard:*
    * Navigate to *Modules > Security Events*.
    * Search for *"suricata"* or *Rule ID: 86601*.
    * *Result:* You should see alerts describing the traffic, such as:
        > *"Suricata: ET OPEN Scanning"*
        > * *Source:* Attacker IP
        > * *Destination:* Kali IP