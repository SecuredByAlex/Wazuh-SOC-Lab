# 🪟 Wazuh Windows Agent Installation & Registration (GUI)

Agent OS: Windows 10 Pro
Wazuh Manager (Ubuntu) IP: 192.168.0.107
Manager Configuration: Ubuntu server already has Wazuh Manager, Indexer, and Dashboard installed.


## 📥 1. Download the Windows Agent

Open a browser on Windows.

Download the latest Wazuh Windows agent MSI from the official source:
```URL
https://packages.wazuh.com/4.x/windows/wazuh-agent-4.x.x.msi
```
📸 Screenshot placeholder: Browser showing download
📸 Screenshot placeholder: File Explorer showing .msi downloaded

## 🛠️ 2. Start GUI Installer

Navigate to the .msi file.

Double-click to open the Wazuh Setup Wizard.
Accept the T&C checkbox and click Install.
After the Installation, Click on finish and run the "Wazuh Agent".

## 🧩 3. Create the Agent on the Ubuntu Manager & Extract Registration Key

Before connecting the Windows agent, the agent must be registered on the manager:

On the Ubuntu Wazuh server, create a new agent entry:

```bash
sudo /var/ossec/bin/manage_agents
```

Select (A)dd agent.

Enter the agent name (e.g., Windows-Agent).

Enter the IP address of the agent (Windows machine).

Copy the generated key shown at the end. This key will be used on Windows to authenticate with the manager.

Example output:

Agent information:
   ID: 001
   Name: Windows-Agent1
   IP: 192.168.0.108
   Key: qwerty12345abcdefghijklmnopqrstuvwxyz


📸 Screenshot placeholder: Terminal showing generated key

## 🌐 4. Configure Manager Connection in GUI Installer

During installation on Windows:

Enter Wazuh Manager IP: 192.168.0.107

Paste the registration key copied from Ubuntu manager into the appropriate field.

Click onn save, then restart the wazuh agent.

The Restart will save the changes and connect the agent with the manger.

📸 Screenshot placeholder: Installer page with Manager IP and registration key

📸 Screenshot placeholder: Windows Services showing Wazuh Agent running


## 🖥️ 5. Confirm Agent on Wazuh Dashboard

Open browser → https://192.168.0.107/

Navigate to Agents → Agent List

Confirm the Windows agent is listed as:

Active

Last Keepalive recent

Correct name and group

📸 Screenshot placeholder: Wazuh Dashboard showing Windows agent