# Wazuh Agent Installation (Windows)

This document describes how to install the *Wazuh Agent* on a Windows machine and connect it to your Ubuntu Wazuh Server using the official deployment method.

---

## 🧰 1. Prerequisites

Ensure your Windows machine meets the following:

- Windows OS (Windows 10)
- Administrator privileges (for PowerShell)
- Network connectivity to the Wazuh Server (192.168.0.108)
- PowerShell 5.0 or higher

---

## 🔐 2. Generate the Installation Command

On your Wazuh Server Dashboard, you need to generate the unique installation script that links the agent to your manager.

1.  Log in to the *Wazuh Dashboard* (https://192.168.0.108/).  
2.  Click *Deploy new agent*.<br>
   <img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/9e97126d-16e3-4685-9626-3b97e379d2dd" /> <br>
3.  Select *Windows* as the Operating System.<br>
4.  Enter the *Wazuh Server IP*: 10.0.2.15 <br>
5.  Assign a group (Default: default). <br>
   <img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/55738616-7f01-4eb8-97a0-bc005a85900d" />
   <img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/136cd473-21c7-42dd-a32f-51da0a3d7dbe" />

This will generate a specific PowerShell command at the bottom of the page. *Copy this command.*

---

## 📦 3. Install the Agent via PowerShell

On the Windows machine, open PowerShell with administrative rights and run the command generated in the previous step.

1.  Search for *PowerShell* in the Start menu.
2.  Right-click and select *Run as Administrator*.
3.  Paste and run the installation command:

```powershell
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.12.0-1.msi -OutFile $env:tmp\wazuh-agent; msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='192.168.0.108' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='windows10-agent' 
```
<br>
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/b5f2a263-c142-4738-9d9a-3d2629f31f6c" /><br>
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/caa6b42a-cbd8-44ab-8b05-c0b52c60ed38" /><br>


(Note: Use the specific command copied from your dashboard, as it contains your unique Manager IP and encryption keys.)
What the script will do:
<ol>
<li> Download the Wazuh Agent MSI package </li>
<li> Install the Wazuh Agent service </li>
<li> Configure the agent to talk to 192.168.0.108 </li>
<li> Register the agent with the manager </li>
</ol>

---

## 🚀 4. Start the Wazuh Agent Service

Once the installation command finishes, you must manually start the service. Run the following command in the same Administrator PowerShell window:

```powershell
NET START Wazuh
```
You should see a message indicating the specific service was started successfully.

---
<br>
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/f75d7fac-6e8e-44dc-bf8e-98a2b7bc8bb5" /><br>

## 🌐 5. Verify Connection

Return to your Wazuh Dashboard to confirm the agent is online.
 * Navigate back to the Agents tab.
 * Refresh the list.
 * Your Windows machine should appear in the list with status Active.
All checks should show the agent is communicating with the manager.

<br>
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/80cccc32-1e9a-4c2e-862c-2354dd64d20e" /><br>
---

## 🎯 6. Installation Completed

Your Windows machine now has a fully functioning:
 * Wazuh Agent Service
 * Real-time Log Collection
 * Threat Detection Capabilities

 ---
