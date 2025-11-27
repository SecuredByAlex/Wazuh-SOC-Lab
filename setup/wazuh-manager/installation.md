# Wazuh Manager Installation (Ubuntu Server)

> **Purpose:** This guide provides step-by-step instructions to install and configure Wazuh Manager, Indexer, and Dashboard on an Ubuntu Server.
> **Applies to:** Ubuntu 20.04 / 22.04 LTS

---

## 🚀 Prerequisites

| Requirement | Details                       |
| ----------- | ----------------------------- |
| OS          | Ubuntu Server 20.04 or 22.04  |
| RAM         | Minimum 4GB (8GB recommended) |
| Storage     | 20GB+                         |
| Access      | Root or sudo privileges       |
| Network     | Internet connection required  |

### **Update System Packages**

```bash
sudo apt update -y && sudo apt upgrade -y
```

### **Install Required Dependencies**

```bash
sudo apt install curl lsb-release gnupg apt-transport-https unzip -y
```

---

## 🏗 Step 1: Add Wazuh Repository

```bash
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo apt-key add -
echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | sudo tee /etc/apt/sources.list.d/wazuh.list
sudo apt update
```

---

## 📦 Step 2: Install Wazuh Indexer

```bash
sudo apt install wazuh-indexer -y
```

### Start and Enable Service

```bash
sudo systemctl daemon-reload
sudo systemctl enable wazuh-indexer
sudo systemctl start wazuh-indexer
```

> **📌 Screenshot Placeholder:** *Indexer installed successfully*

---

## 🔧 Step 3: Configure Indexer (Basic Setup)

> The full configuration is stored in `indexer.yml` (added separately in repo)

Restart service after editing configuration:

```bash
sudo systemctl restart wazuh-indexer
```

---

## 🛡 Step 4: Install Wazuh Manager

```bash
sudo apt install wazuh-manager -y
```

Start service:

```bash
sudo systemctl enable wazuh-manager
sudo systemctl start wazuh-manager
```

> **📌 Screenshot Placeholder:** *Manager service running*

---

## 🌐 Step 5: Install Wazuh Dashboard

```bash
sudo apt install wazuh-dashboard -y
sudo systemctl enable wazuh-dashboard
sudo systemctl start wazuh-dashboard
```

> **📌 Screenshot Placeholder:** *Dashboard installation and login*

---

## 🔑 Step 6: Access the Dashboard

Open browser:

```
https://<server-ip>:5601
```

Default login (if not changed):

```
user: admin
pass: admin
```

⚠ Change password after first login.

> **📌 Screenshot Placeholder:** *Dashboard home screen*

---

## 🧪 Step 7: Verify Services

```bash
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-dashboard
```

> **📌 Screenshot Placeholder:** *Service status output*

---

## 🤝 Step 8: Add an Agent (Example - Windows)

```bash
/var/ossec/bin/manage_agents
```

Select `Add agent`, generate key, copy it to the endpoint.

> Detailed agent setup files are stored in:

```
/setup/windows-agent/
/setup/kali-agent/
```

> **📌 Screenshot Placeholder:** *Agent enrollment process*

---

## ✔ Final Validation Checklist

| Task                  | Status |
| --------------------- | ------ |
| Wazuh Indexer running | 🔲     |
| Wazuh Manager running | 🔲     |
| Dashboard accessible  | 🔲     |
| Agent enrolled        | 🔲     |
| Alerts visible        | 🔲     |

---

## 📁 Related Files

| File                  | Purpose                          |
| --------------------- | -------------------------------- |
| `ossec.conf`          | Core Wazuh manager config        |
| `indexer.yml`         | Indexer storage + cluster config |
| `dashboards-setup.md` | Custom dashboards setup          |

---

## 📝 Notes

* For production, enable SSL and configure authentication.
* Ensure sufficient RAM for indexing logs.
* Backup configuration files before upgrades.

---

*End of installation guide.*
