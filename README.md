# 🛡️Wazuh SIEM SOC Home Lab
Windows + Kali + Ubuntu Server<br>


## 📌 Project Overview
This project demonstrates how a SOC Analyst builds, configures, and uses a SIEM (Wazuh) to detect security events. The lab consists of a Wazuh server on Ubuntu, and two monitored endpoints: a Windows 10 VM and a Kali Linux VM. The goal of the project is to simulate attacks, collect logs and generate SOC-style incident reports.


## 🧱 Lab Architecture

<img width="796" height="462" alt="Image" src="https://github.com/user-attachments/assets/d429d921-cc0b-48e2-89d6-8bd977b103b0" />

## 🖥 OS & Tool Versions

| Component           | Version          |
|--------------------|------------------|
| Wazuh Manager      | Latest Stable    |
| Ubuntu Server      | 24.04            |
| Windows Agent      | Windows 10 Pro   |
| Linux Agent        | Kali Linux 2024.4|
| Filebeat / Indexer | Latest Stable    |
| OpenSearch Dashboards | Latest Stable |

   
## 🧪 Labs Implemented

### Lab 1: File Integrity Monitoring (FIM)
* **Goal:** Detect unauthorized file creation, modification, or deletion in critical directories.
* **Configuration:** Enabled real-time `syscheck` monitoring on the Agent's `/root` directory.
* **Test:** Created a specific artifact file to trigger the "File Added to System" alert.

### Lab 2: Network Intrusion Detection (Suricata)
* **Goal:** Detect network-based attacks like port scanning.
* **Configuration:** Installed Suricata on the endpoint and configured Wazuh to ingest `eve.json` logs.
* **Test:** Launched an Nmap scan from Kali Linux to generate "ET Open: Scanning" alerts on the Dashboard.

### Lab 3: Vulnerability Detection
* **Goal:** Automate the discovery of known vulnerabilities (CVEs) in installed software.
* **Configuration:** Enabled the `vulnerability-detector` module in `ossec.conf` to cross-reference installed packages with the NVD database.
* **Test:** Identified outdated packages and critical CVEs on the Ubuntu agent.

### Lab 4: Malicious Command Detection (Auditd)
* **Goal:** Monitor user behavior and specific suspicious commands (e.g., `netstat`, `whoami`) rather than just malware signatures.
* **Configuration:** Installed `auditd` on Linux and configured rules to watch for specific system calls and command executions.
* **Test:** Executed suspicious commands as the root user to verify log capture and alert generation.

### Lab 5: SSH Brute Force & Active Response
* **Goal:** Detect repeated failed login attempts and automatically block the attacker.
* **Configuration:** Configured an `<active-response>` block in the Wazuh Manager to trigger a firewall drop command.
* **Test:** Simulated an SSH brute force attack using Hydra; verified that the IP was automatically blocked by the firewall for 180 seconds.

### Lab 6: Threat Intelligence Integration (VirusTotal)
* **Goal:** Enhance detection capabilities by integrating external threat intelligence APIs.
* **Configuration:** Connected the VirusTotal API to Wazuh to automatically scan file hashes.
* **Test:** Downloaded a test malware file (EICAR) to verify that Wazuh queried VirusTotal and flagged the file as malicious.
