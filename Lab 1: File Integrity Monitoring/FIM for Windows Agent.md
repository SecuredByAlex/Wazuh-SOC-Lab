# 📂 Lab 1: File Integrity Monitoring (Windows Agent)
*Goal:* Detect file creation on the Desktop Folder in real-time.

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
    ```bash
    <directories check_all="yes" report_changes="yes" realtime="yes">C:\File Integrity</directories>
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/e7eaf272-3b3f-4827-b71b-ae075ed01e1c" />
    
4.  *Save the file.*
5.  *Restart the Wazuh Service:*
    * Open Wazuh Agent -> Manage Tab -> Restart

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/a986ba86-4ea2-4971-8a9d-30303d5a035e" /><br>
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/5c52c3d7-d62e-43b8-ae90-6abfece60bd4" />

---

## 🔹 Part 3: Verification & Results
Trigger the alert and verify it on the Dashboard.

1.  *Trigger the Event:*
    * Go to your File integrity Folder.
    * Right-click > New > *Text Document*.
    * Name it: stolen_passwords.txt.
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/9232ff90-32ff-437b-8a0b-2d77dac8f1c7" />  <br>

2.  *Check Wazuh Dashboard:*
    * Navigate to *Modules > Security Events*.
    * Search for *Rule ID: 554* (File added).
    * *Result:* You should see an alert with the description:
        > *"File added to the system"*
        > * *File:* C:\File Integrity\stolen_passwords.txt
        > * *Agent:* Windows-10-Agent
        
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/dd3e8f1a-9c63-4df9-b5c5-21faa83690cc" />
