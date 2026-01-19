# 📂 Lab 1: File Integrity Monitoring (Kali Linux Agent)
*Goal:* Detect unauthorized file creation, modification, or deletion in the /root directory in real-time.

---

## 🔹 Part 1: Server-Side Configuration Check (Wazuh Manager)
Before configuring the agent, ensure the FIM module (syscheck) is enabled on the Manager.

1.  *Access the Wazuh Manager* (via CLI or SSH).
2.  *Open the Configuration File:*
    ```bash
    sudo nano /var/ossec/etc/ossec.conf
    ```
    <img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/0b032bd0-db47-47fe-bb89-4b58548246df" />
    
3.  *Verify global is Enabled:*
    * Scroll down to the (logall) is set to yes.
    * Ensure the disabled tag in the syscheck of the "File Integrity Monitoring" is set to no.
    
    ```bash
    <logall>yes</logall>
    <logall_json>yes</logall_json>
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/98bf18ac-685e-471f-a674-d5e4666a2c49" />

    ```bash
    <!-- File integrity monitoring -->
    <syscheck>
      <disabled>no</disabled>
      ...
    </syscheck>
    ```
 <img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/44540d52-ceb8-4e0e-809f-2b6c7f3fc02e" />

   (Note: This is usually enabled by default).
   
4.  *Save & Exit* if you made changes, and restart the manager:
    ```bash
    sudo systemctl restart wazuh-manager
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/fdd95ca8-5bb7-4a7b-9a1b-e019f51f3235" />

---

## 🔹 Part 2: Agent Configuration (Kali Linux)
Configure the specific directory we want to monitor.

1.  *Access the Kali Agent* terminal.
2.  *Edit the Agent Configuration:*
    
    ```bash
    sudo nano /var/ossec/etc/ossec.conf
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/5862738e-afc2-482d-bab9-765962bd4ba4" />
    
3.  *Add the Directory to Monitor:*
    * Find the syscheck block.
    * Add the following line to monitor the /root directory with real-time reporting:
    
    ```bash
    <directories check_all="yes" report_changes="yes" realtime="yes">/root</directories>
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/4c85759a-15e7-4663-9237-c49ef117d2cd" />
    
4.  *Save the file* (Ctrl+O, Enter, Ctrl+X).
5.  *Restart the Agent* to apply changes:
    
    ```bash
    sudo systemctl restart wazuh-agent
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/f5dc3b39-8ac9-490e-a807-1cfde41ff702" />

---

## 🔹 Part 3: Verification & Results
Trigger the alert and verify it on the Dashboard.

1.  *Trigger the Event:*
    * On the Kali terminal, create a new "malicious" file in the monitored directory:
    ```bash
    sudo touch /root/malicious_backdoor.sh
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/e2c0b751-a460-451c-b486-39d36f3be1ec" />
    
2.  *Check Wazuh Dashboard:*
    * Navigate to *Modules > Security Events*.
    * Search for *Rule ID: 554* (File added to the system).
    * *Result:* You should see an alert with the description:
        > *"File added to the system"*
        > * *File:* /root/malicious_backdoor.sh
        > * *Agent:* Kali-Linux
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/c78fef10-e9cb-44ab-8196-7dd776766317" />
        
