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
    
3.  *Verify syscheck is Enabled:*
    * Scroll down to the <syscheck> section.
    * Ensure the <disabled> tag is set to no.
    
    ```bash
    <logall>yes</logall>
    <logall_json>yes</logall_json>
    ```

    ```bash
    <!-- File integrity monitoring -->
    <syscheck>
      <disabled>no</disabled>
      <frequency>43200</frequency>
      <scan_on_start>yes</scan_on_start>
      ...
    </syscheck>
    ```

    (Note: This is usually enabled by default).
4.  *Save & Exit* if you made changes, and restart the manager:
    ```bash
    sudo systemctl restart wazuh-manager
    ```

---

## 🔹 Part 2: Agent Configuration (Kali Linux)
Configure the specific directory we want to monitor.

1.  *Access the Kali Agent* terminal.
2.  *Edit the Agent Configuration:*
    
    ```bash
    sudo nano /var/ossec/etc/ossec.conf
    ```
    
3.  *Add the Directory to Monitor:*
    * Find the <syscheck> block.
    * Add the following line to monitor the /root directory with real-time reporting:
    
    ```bash
    <directories check_all="yes" report_changes="yes" realtime="yes">/root</directories>
    ```
    
4.  *Save the file* (Ctrl+O, Enter, Ctrl+X).
5.  *Restart the Agent* to apply changes:
    
    ```bash
    sudo systemctl restart wazuh-agent
    ```

---

## 🔹 Part 3: Verification & Results
Trigger the alert and verify it on the Dashboard.

1.  *Trigger the Event:*
    * On the Kali terminal, create a new "malicious" file in the monitored directory:
    ```bash
    sudo touch /root/malicious_backdoor.sh
    ```
    
2.  *Check Wazuh Dashboard:*
    * Navigate to *Modules > Security Events*.
    * Search for *Rule ID: 554* (File added to the system).
    * *Result:* You should see an alert with the description:
        > *"File added to the system"*
        > * *File:* /root/malicious_backdoor.sh
        > * *Agent:* Kali-Linux