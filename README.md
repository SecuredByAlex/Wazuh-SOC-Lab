# 🛡️Wazuh SIEM SOC Home Lab
Windows + Kali + Ubuntu Server<br>
Threat Detection | Sysmon | Sigma Rules | Attack Simulation

## 📌 Project Overview
This project demonstrates how a SOC Analyst builds, configures, and uses a SIEM (Wazuh) to detect security events. The lab consists of a Wazuh server on Ubuntu, and two monitored endpoints: a Windows 10 VM and a Kali Linux VM. The goal of the project is to simulate attacks, collect logs, create Sigma rules, and generate SOC-style incident reports.


## 🧱 Lab Architecture




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
