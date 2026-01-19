# 📂 Lab 1: File Integrity Monitoring (Windows Agent)
*Goal:* Detect unauthorized file creation on the Administrator's Desktop in real-time.

---

## 🔹 Part 1: Server-Side Configuration Check (Wazuh Manager)
Before configuring the agent, ensure the FIM module (syscheck) is enabled on the Manager.

1.  *Access the Wazuh Manager*.
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

## 🔹 Part 2: Agent Configuration (Windows 10)
Configure the Windows Agent to watch a specific user folder.

1.  *Open Notepad as Administrator:*
    * Search for "Notepad" -> Right Click -> *Run as Administrator*.
2.  *Open the Config File:*
    * File > Open.
    * Path: C:\Program Files (x86)\ossec-agent\ossec.conf.
    * Tip: Change the file type dropdown from "Text Documents" to "All Files" to see the .conf file.
3.  *Add the Directory to Monitor:*
    * Locate the <syscheck> section.
    * Add the line below (Replace YourUserName with your actual Windows user, e.g., Admin):
    xml
    <directories check_all="yes" report_changes="yes" realtime="yes">C:\File Integrity</directories>
    
4.  *Save the file.*
5.  *Restart the Wazuh Service:*
    * Open Wazuh Agent -> Manage Tab -> Restart
    
    

---

## 🔹 Part 3: Verification & Results
Trigger the alert and verify it on the Dashboard.

1.  *Trigger the Event:*
    * Go to your File integrity Folder.
    * Right-click > New > *Text Document*.
    * Name it: stolen_passwords.txt.
2.  *Check Wazuh Dashboard:*
    * Navigate to *Modules > Security Events*.
    * Search for *Rule ID: 554* (File added).
    * *Result:* You should see an alert with the description:
        > *"File added to the system"*
        > * *File:* C:\Users\YourUserName\Desktop\stolen_passwords.txt
        > * *Agent:* Windows-10-Agent