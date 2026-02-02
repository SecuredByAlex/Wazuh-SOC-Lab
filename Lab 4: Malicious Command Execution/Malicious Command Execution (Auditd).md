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

2.  *Add Log Collection Block:*
    Check if the <localfile> block for audit.log exists. If not, add this block inside the <ossec_config> section:
    ```bash
    <localfile>
      <log_format>audit</log_format>
      <location>/var/log/audit/audit.log</location>
    </localfile>
    ```

3.  *Restart Wazuh Agent:*
    ```bash
    sudo systemctl restart wazuh-agent
    ```

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



3.  *Verify Rules:*
    Check if the rules are active:
   ```bash
    sudo auditctl -l
    ```

---

## 🔹 Part 3: Verification & Results
Simulate a malicious command and check the dashboard.

1.  *Trigger the Event (The Attack):*
    * As the *Root* user (or using sudo), run a suspicious command like netstat or view a sensitive file.
    * Command:
    ```bash
    sudo netstat -tulpn
    ```

2.  *Check Wazuh Dashboard:*
    * Navigate to *Discover*.
    * Search for *"audit"* or filter by rule.groups: audit.
    * *Result:

    