# Wazuh Agent Installation (Kali - Linux)

This document describes how to install the *Wazuh Agent* on a Kali-Linux machine and connect it to your Ubuntu Wazuh Server using the official deployment method.

---

## 🧰 1. Prerequisites

Ensure your Windows machine meets the following:

- Kali Linux (Rolling edition recommended)
- Root or Sudo privileges
- Network connectivity to the Wazuh Server (10.0.2.15)
- Updated system:

```bash
sudo apt update -y
```
---

## 🔐 2. Generate the Installation Command

On your Wazuh Server Dashboard, you need to generate the installation script.
 * Log in to the Wazuh Dashboard (https://192.168.0.108/).
 * Click the Wazuh menu icon (▼) and select Agents.
 * Click Deploy new agent.
 * Select Debian/Ubuntu as the Operating System (Kali is Debian-based).
 * Enter the Wazuh Server IP: 192.168.0.108
 * Assign a group (Default: default).

 This will generate a command string at the bottom. Copy this command.

---

## 📦 3. Install the Agent via Terminal

On your Kali Linux machine, open your terminal and paste the command you generated.

```bash
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.12.0-1_amd64.deb && sudo WAZUH_MANAGER='192.168.0.108' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='Kali-agent' dpkg -i ./wazuh-agent_4.12.0-1_amd64.deb
```
//ss

(Note: Use the specific command copied from your dashboard, as it contains your unique Manager IP and encryption keys.)
What the script will do:
<ol>
<li> Download the Wazuh Agent DEB package </li>
<li> Install the Wazuh Agent service </li>
<li> Configure the agent to talk to 192.168.0.108 </li>
<li> Register the agent with the manager </li>
</ol>

---

## 🚀 4. Start the Wazuh Agent Service

Once the installation command finishes, you must manually start the service. Run the following command in the same Terminal window:

```bash
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```
//ss

---

## 🌐 5. Verify Connection

Return to your Wazuh Dashboard to confirm the agent is online.
 * Navigate back to the Agents tab.
 * Refresh the list.
 * Your Kali machine should appear in the list with status Active.
All checks should show the agent is communicating with the manager.


---

# 🎯 6. Installation Completed

Your Windows machine now has a fully functioning:
 * Wazuh Agent Service
 * Real-time Log Collection
 * Threat Detection Capabilities

 ---
