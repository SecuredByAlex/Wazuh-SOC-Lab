# 🛡️ Lab 4: Malicious Command Detection (Auditd)
*Goal:* Monitor user behavior and specific suspicious commands (e.g., netstat, whoami) executed by the root user using the Linux Audit daemon (auditd).

---

## 🔹 Part 1: Agent Configuration (Kali Linux)
Install Auditd and configure the Wazuh Agent to forward the logs.

### Step A: Install Auditd
1.  *Install the Auditd package:*
    Open your Kali terminal and run:
    ```bash
    sudo apt install auditd -y
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/1c89e9c7-48f7-4f57-8738-13baf89a8499" />

2.  *Verify the Log File:*
    Ensure the log file exists (this is where auditd stores events):
    ```bash
    ls -l /var/log/audit/audit.log
    ```

### Step B: Configure Wazuh Agent
1.  *Edit the Agent Config:*
    ```bash
    sudo nano /var/ossec/etc/ossec.conf
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/e46be987-966a-41bd-928d-8b0aef86a8ee" />

2.  *Add Log Collection Block:*
    Check if the <localfile> block for audit.log exists. If not, add this block inside the <ossec_config> section:
    ```bash
    <localfile>
      <log_format>audit</log_format>
      <location>/var/log/audit/audit.log</location>
    </localfile>
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/9ebe93ea-b16d-43e0-9626-a61ab625c3ad" />

3.  *Restart Wazuh Agent:*
    ```bash
    sudo systemctl restart wazuh-agent
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/276a6a82-0882-4abf-825b-fa02cdd83ba1" />

---

## 🔹 Part 2: Define Audit Rules
Configure Auditd to watch for specific "Hacker Behavior" (e.g., commands run by Root).

1.  *Edit Audit Rules:*
    Open the rules file:
    ```bash
    sudo nano /etc/audit/rules.d/audit.rules
    ```

2.  *Add Monitoring Rules:*
    Paste the following rule at the bottom of the file. This rule tracks any command execution (execve) by a user with Root privileges (euid=0).
    
    * *Rule Breakdown:*
        * -a always,exit: Always record the event upon exit.
        * -F arch=b64: Filter for 64-bit architecture.
        * -F euid=0: Filter for Root user (Effective User ID 0).
        * -S execve: Monitor the "Execution" system call.
        * -k audit-wazuh-c: Tag the event with a key (Must match Wazuh's CDB list).

    ```bash
    -a always,exit -F arch=b64 -F euid=0 -S execve -k audit-wazuh-c
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/d335abe3-12e6-4169-861a-f7fdc0ad2b3c" />

3.  *Verify Rules:*
    Check if the rules are active:
    ```bash
    sudo auditctl -l
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/d97fff73-fd52-45a2-bd27-875737f63e54" />

---

## 🔹 Part 3: Verification & Results
Simulate a malicious command and check the dashboard.

1.  *Trigger the Event (The Attack):*
    * As the *Root* user (or using sudo), run a suspicious command like netstat or view a sensitive file.
    * Command:
    ```bash
    sudo netstat -tulpn
    ```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/58d0fbdb-90b9-4d51-bae9-c456ae5fe1e0" />

2.  *Check Wazuh Dashboard:*
    * Navigate to *Discover*.
    * Search for *"audit"* or filter by rule.groups: audit.
    * *Result:*
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/d0527ccc-66a0-48b9-8b40-fac3795ec8e8" />


    
