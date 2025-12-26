# Wazuh Server Installation (Ubuntu)

This document describes how to install the **Wazuh Server** on an Ubuntu VirtualBox machine using the official Wazuh installation script.

---

## 🧰 1. Prerequisites

Ensure your Ubuntu VM meets the following:

- 64-bit Ubuntu OS (20.04 / 22.04 / 24.04 recommended)
- Internet connectivity  
- Sudo privileges  
- Updated system:

```bash
sudo apt update -y && sudo apt upgrade -y
```

## 🔐 2. Add the Wazuh GPG Key

Run the following command to download and store the GPG key used to validate Wazuh packages:

```bash
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --dearmor -o /usr/share/keyrings/wazuh-archive-keyring.gpg
```

This securely adds the GPG key to your system.

## 📦 3. Download & Run the Wazuh Installation Script

Download the official Wazuh installer and run it:

```bash
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i
```

Explanation of flags:

  -  -a → Installs all components:

       - Wazuh Manager
       - Wazuh Indexer
       - Wazuh Dashboard

  -  -i → Runs in interactive mode (lets you confirm steps)

What the installer will do:
<ol>
   <li> Configure repositories </li>
   <li> Install all required services </li>
   <li> Generate TLS certificates </li>
   <li> Set up the indexer cluster (single-node by default) </li>
   <li> Install and configure the dashboard </li>
</ol>

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/da534378-d11d-4d33-8f56-2cb1bd48a17d" />

## 🚀 4. Start and Verify Wazuh Services

Run the following commands to confirm that the services are active:

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```


<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/0b4ce1e0-727b-4671-8b78-125bac729aa8" />

All services should show active (running).

## 🌐 5. Access Wazuh Dashboard

Open your browser and visit:

https://10.0.2.15/

Default credentials are shown at the end of the installation script.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/2f701bf2-7426-4648-b567-7e57240e600e" />

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/a71392c5-5623-4f94-b5ad-c89600fe3d93" />

## 🎯 6. Installation Completed

Your Ubuntu server now has a fully functioning:

   - Wazuh Manager

   - Wazuh Indexer

   - Wazuh Dashboard
     

