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
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH \
    | sudo gpg --dearmor -o /usr/share/keyrings/wazuh-archive-keyring.gpg
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

    - Configure repositories
    - Install all required services
    - Generate TLS certificates
    - Set up the indexer cluster (single-node by default)
    - Install and configure the dashboard

ss: username and pass
## 🚀 4. Start and Verify Wazuh Services

Run the following commands to confirm that the services are active:

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

All services should show active (running).


## 🌐 5. Access Wazuh Dashboard

Open your browser and visit:

https://<your-ubuntu-ip>:5601

Default credentials are shown at the end of the installation script.

📸 Screenshot placeholder: Wazuh Dashboard login page.
## 🎯 6. Installation Completed

Your Ubuntu server now has a fully functioning:

   - Wazuh Manager

   - Wazuh Indexer

   - Wazuh Dashboard