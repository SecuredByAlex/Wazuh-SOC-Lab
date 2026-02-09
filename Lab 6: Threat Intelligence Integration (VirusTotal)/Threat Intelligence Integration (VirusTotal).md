# 🛡️ Lab 6: Threat Intelligence Integration (VirusTotal)
**Goal:** Enhance detection capabilities by integrating the VirusTotal API. Wazuh will automatically scan the hash of any file added to the `/root` directory against the VirusTotal database to detect malware.

---

## 🔹 Part 1: Get Your API Key
*You need a free VirusTotal account to allow Wazuh to query their database.*

1.  **Register:** Go to [VirusTotal.com](https://www.virustotal.com/) and sign up for a free account.
2.  **Get Key:** Click your profile icon (top right) > **API Key**.
3.  **Copy:** Copy your alphanumeric `API Key`.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/9375462d-e29e-46f0-b85a-1278ce59065f" />

---

## 🔹 Part 2: Create a Custom Rule (Wazuh Manager)
*We will create a specific rule to trigger VirusTotal **ONLY** for files in the `/root` directory. This prevents us from hitting the API rate limit (4 requests/minute).*

1.  **Edit Local Rules:**
    On your **Ubuntu Manager**:
    ```bash
    sudo nano /var/ossec/etc/rules/local_rules.xml
    ```

2.  **Add the Filter Rule:**
    Paste this block inside the `<group name="local,syslog,">` section (before the `</group>` tag):

    ```xml
    <rule id="100200" level="5">
      <if_sid>554</if_sid>
      <field name="file">/root</field>
      <description>File added to /root directory.</description>
    </rule>
    ```
    * **Logic:** If FIM detects a file addition (Rule 554) **AND** the file path is `/root`, trigger Rule 100200.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/8bcfe1f4-e028-47e7-a08a-3c5faa68a8f4" />

3.  **Save & Exit:** (`Ctrl+O`, `Enter`, `Ctrl+X`).

---

## 🔹 Part 3: Enable Integration (Wazuh Manager)
*Configure the Manager to send alerts from Rule 100200 to VirusTotal.*

1.  **Edit Config:**
    ```bash
    sudo nano /var/ossec/etc/ossec.conf
    ```

2.  **Add Integration Block:**
    Scroll to the bottom (outside any other block) and paste this.
    * ⚠️ **Replace `<YOUR_VT_API_KEY>` : `61bf55d83c68fc865eb0ae923f37d2627dc1887ea77959939366560ebb8470d8` with your actual key.** <br>
    

    ```xml
    <integration>
      <name>virustotal</name>
      <api_key>61bf55d83c68fc865eb0ae923f37d2627dc1887ea77959939366560ebb8470d8</api_key>
      <rule_id>100200</rule_id>
      <alert_format>json</alert_format>
    </integration>
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/e4fb087c-4216-48ea-9a24-c59db4686288" />

3.  **Restart Manager:**
    ```bash
    sudo systemctl restart wazuh-manager
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/86ec750e-2343-4e16-8def-ddc5dd96852a" />

---

## 🔹 Part 4: Attack Simulation (Kali Linux)
*Download a harmless test virus (EICAR) to trigger the system.*

1.  **Access Kali Terminal.**
2.  **Download Malware:**
    Run this command to download the EICAR test file into the monitored folder:
    ```bash
    sudo curl -Lo /root/eicar.com https://secure.eicar.org/eicar.com
    ```
    *(Note: The FIM system might take a few seconds to detect the file creation).*

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/b5f6a977-1031-4072-80aa-eac95b6c6825" /><br>
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/43c4062f-bda9-4e24-b803-13c860f03750" />

---

## 🔹 Part 5: Verification & Results
*Verify that Wazuh detected the file and flagged it as malware.*

 **Check Wazuh Dashboard:**
    * Navigate to **Modules > Security Events**.
    * Search for **"virustotal"**.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/aab6a3d6-ad39-4fca-be6a-437e48edf163" />
