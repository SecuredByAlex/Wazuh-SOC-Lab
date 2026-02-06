# 🛡️ Lab 5: SSH Brute Force & Active Response
*Goal:* Detect repeated failed login attempts (Brute Force) on the Ubuntu Agent and automatically block the attacker's IP using Wazuh Active Response.

---

## 🔹 Part 1: Server-Side Configuration (Wazuh Manager)
Configure the Active Response module to trigger a firewall drop when an attack is detected.

### Step A: Verify the Command
1.  *Edit the Manager Config:*
    Open the configuration file on your *Ubuntu Manager*:
    ```bash
    sudo nano /var/ossec/etc/ossec.conf
    ```

2.  *Check for firewall-drop Command:*
    Scroll down to the <command> section and ensure this block exists (it is usually there by default):
    ```bash
    <command>
      <name>firewall-drop</name>
      <executable>firewall-drop</executable>
      <timeout_allowed>yes</timeout_allowed>
    </command>
    ```

### Step B: Configure Active Response
1.  *Add the Response Block:*
    Scroll to the <active-response> section. Add the following block to tell Wazuh to block the IP for 180 seconds if Rule ID *5763* (SSH Brute Force) is triggered:
    
    ```bash
    <active-response>
      <command>firewall-drop</command>
      <location>local</location>
      <rules_id>5763</rules_id>
      <timeout>180</timeout>
    </active-response>
    ```

2.  *Save & Restart:*
    * Save the file (Ctrl+O, Enter, Ctrl+X).
    * Restart the Manager:
        ```bash
        sudo systemctl restart wazuh-manager
        ```

---

## 🔹 Part 2: Attack Simulation (Kali Linux)
Use Hydra to simulate a brute force attack against the Ubuntu Agent.

1.  *Install Hydra (if not installed):*
    On your Kali machine:
    ```bash
    sudo apt install hydra -y
    ```

2.  *Launch the Attack:*
    Run the following command to try multiple passwords against the Wazuh Manager (or Agent).
    * Replace <TARGET_IP> with your Ubuntu Manager/Agent IP (e.g., 192.168.0.108).
    * We use a dummy password list for the test.

    ```bash
    hydra -l wazuh -P /usr/share/wordlists/rockyou.txt ssh://<TARGET_IP>
    ```
    (Note: You can also use a small custom text file with random passwords if rockyou.txt is too large).

---

## 🔹 Part 3: Verification & Results
Verify that Wazuh detected the attack and blocked the IP.

1.  *Check Wazuh Dashboard:*
    * Navigate to *Modules > Security Events*.
    * Search for *Rule ID: 5763* or text *"Host blocked by firewall-drop"*.

2.  *Confirm the Block:*
    * *Alert 1:* You should see multiple alerts for "SSHD brute force trying to get access to the system" (Rule 5763).
    * *Alert 2:* You should see an Active Response alert: "Host blocked by firewall-drop".

3.  *Verify Connectivity Loss:*
    * From your Kali machine, try to ping the Target IP.
    * It should *fail* (Request Timed Out) for 180 seconds, proving the firewall rule is active.