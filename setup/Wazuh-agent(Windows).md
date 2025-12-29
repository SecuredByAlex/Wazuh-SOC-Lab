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
2.  Click *Deploy new agent*.
3.  Select *Windows* as the Operating System.
4.  Enter the *Wazuh Server IP*: 10.0.2.15
5.  Assign a group (Default: default).

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
//ss for powershell start finish.

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

## 🌐 5. Verify Connection

Return to your Wazuh Dashboard to confirm the agent is online.
 * Navigate back to the Agents tab.
 * Refresh the list.
 * Your Windows machine should appear in the list with status Active.
All checks should show the agent is communicating with the manager.

---

## 🎯 6. Installation Completed

Your Windows machine now has a fully functioning:
 * Wazuh Agent Service
 * Real-time Log Collection
 * Threat Detection Capabilities

 ---