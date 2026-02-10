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
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/ed9fa8a6-5ab3-4b8c-b905-1da2e6760c56" />

2. Download and extract the Emerging Threats Suricata ruleset:
```bash
cd /tmp/ && curl -LO https://rules.emergingthreats.net/open/suricata-6.0.8/emerging.rules.tar.gz
sudo tar -xvzf emerging.rules.tar.gz && sudo mv rules/*.rules /etc/suricata/rules/
sudo chmod 640 /etc/suricata/rules/*.rules
```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/53355df7-4972-47e1-85bf-73d06ce584cc" />

3. Modify Suricata settings in the /etc/suricata/suricata.yaml file and set the following variables:
```bash
sudo nano /etc/suricata/suricata.yaml
```

```JavaScript
HOME_NET: "10.0.2.15"
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
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/bec1d17d-857f-48bd-b5d8-494f43f0416c" /><br>
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/ae2244b9-0b7c-4c66-9c00-d2a79691961f" /><br>
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/fdfe1f46-783a-4802-a1ca-611e81a0c5ac" /><br>
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/bad08b7f-b553-4bc3-ac13-5f7c24cfb5c2" /><br>

4. Restart the Suricata service:
```bash
sudo systemctl restart suricata
```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/dad2b40b-4818-4d3d-9813-7af9c14743ad" />

### Step B: Configure Wazuh Agent

1.  *Edit Agent Config:*
    ```bash
    sudo nano /var/ossec/etc/ossec.conf
    ```

2.  *Add Log Collection Block:*
    * Add this block inside the <ossec_config> section to read Suricata's EVE JSON log:
    ```bash
    <localfile>
      <log_format>json</log_format>
      <location>/var/log/suricata/eve.json</location>
    </localfile>
    ```
  <img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/02719530-9b9e-4568-bd27-574ed3dac0d5" />
    
3.  *Restart Wazuh Agent:*
    ```bash
    sudo systemctl restart wazuh-agent
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/32314d7f-3a56-435e-b061-276f48ad3bda" />

---

## 🔹 Part 2: Verification & Results
*Simulate a malicious network signature to trigger the IDS.*

1.  **Trigger the Event:**
    * On the Kali machine (the one running Suricata), run a command to download a fake malicious artifact. This specific URL returns a response that looks like a successful root compromise (`uid=0(root)`), which triggers a high-severity IDS rule.
    ```bash
    curl http://testmyids.com
    ```
<img width="1920" height="1032" alt="Image" src="https://github.com/user-attachments/assets/e6065353-34f5-4a79-90a9-dd74649f2782" />

2.  **Check Wazuh Dashboard:**
    * Navigate to **Modules > Security Events**.
    * Filter by `rule.groups: suricata`.
    * **Result:** You should see a critical alert indicating a potential system compromise.
    * **Alert Name:** `Suricata: Alert - GPL ATTACK_RESPONSE id check returned root`
<img width="1920" height="1032" alt="Image" src="https://github.com/user-attachments/assets/ab8ca546-ef26-4b6d-bf94-1204c9b7a37f" />


