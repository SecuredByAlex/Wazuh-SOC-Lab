# 🛡️Wazuh SIEM SOC Home Lab
Windows + Kali + Ubuntu Server<br>
Threat Detection | Sysmon | Sigma Rules | Attack Simulation

## 📌 Project Overview
This project demonstrates how a SOC Analyst builds, configures, and uses a SIEM (Wazuh) to detect security events. The lab consists of a Wazuh server on Ubuntu, and two monitored endpoints: a Windows 10 VM and a Kali Linux VM. The goal of the project is to simulate attacks, collect logs, create Sigma rules, and generate SOC-style incident reports.


## 🧱 Lab Architecture

<img width="796" height="462" alt="Image" src="https://github.com/user-attachments/assets/3a488e9b-0f65-4364-ba08-dac44ea0a72d" />

## 🖥 OS & Tool Versions

| Component           | Version          |
|--------------------|------------------|
| Wazuh Manager      | Latest Stable    |
| Ubuntu Server      | 25.04            |
| Windows Agent      | Windows 10 Pro   |
| Linux Agent        | Kali Linux 2024.4|
| Filebeat / Indexer | Latest Stable    |
| OpenSearch Dashboards | Latest Stable |

## ⚙️ Setup Overview

1. Install and configure Wazuh Manager on Ubuntu 25.04
2. Configure Filebeat / Wazuh Indexer for log forwarding and indexing
3. Install Wazuh Agent on Windows 10 Pro
4. Enable Windows event logs and PowerShell monitoring
5. Install Wazuh Agent on Kali Linux 2024.4
6. Enable syslog, authentication logs, and command monitoring on Kali
7. Connect both agents to the manager and verify enrollment
8. Create dashboards for monitoring events
9. Run attack simulations on Windows and Kali
10. Analyze alerts generated in Wazuh Dashboard
   
## 🔍 Simulated Attacks Performed

- SSH Brute Force
- Password Spraying
- Suspicious PowerShell Execution
- Privilege Escalation Attempt
- Nmap Scanning
- Malicious File Execution


## 🛡 Custom Detection Rules
- Multiple Failed SSH Login Attempts (Linux)
- Suspicious PowerShell Execution (Windows)


## 📊 Dashboards Created

- Authentication Events
- Failed Login Attempts
- Windows Event Monitoring
- Linux Syslog Monitoring
- PowerShell Activity
- Threat Detection Overview
