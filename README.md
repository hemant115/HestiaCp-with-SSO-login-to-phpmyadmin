```markdown
# HestiaCP phpMyAdmin SSO and API Access Guide

This document provides a comprehensive guide to installing HestiaCP,
enabling and troubleshooting phpMyAdmin Single Sign-On (SSO) and verifying API access.
It also includes a quick test to validate SSO functionality across multiple databases.

---

## 📚 Table of Contents

1. [HestiaCP Installation](#1-hestiacp-installation)  
2. [Enabling phpMyAdmin SSO](#2-enabling-phpmyadmin-sso)  
3. [Troubleshooting phpMyAdmin SSO](#3-troubleshooting-phpmyadmin-sso)  
4. [Verifying HestiaCP API Access](#4-verifying-hestiacp-api-access)  
5. [SSO Functionality Test](#5-sso-functionality-test)

---

## 1. HestiaCP Installation

### 1.1 Preparing the Server

- Use a fresh **Debian** or **Ubuntu** server (RHEL-based systems are not supported).
- Ensure **root access** and **SSH connectivity**.
- Update and upgrade the system:

```bash
sudo apt update && sudo apt upgrade -y && apt-get install ca-certificates
```

### 1.2 Downloading and Running the Installation Script

- Download the official installation script:

```bash
wget https://raw.githubusercontent.com/hestiacp/hestiacp/release/install/hst-install.sh
```

- Run the script with desired components:

```bash
sudo bash hst-install.sh --apache yes --nginx yes --phpfpm yes --mysql yes --exim yes --dovecot yes --clamav yes --spamassassin yes --iptables yes --fail2ban yes
```

- Follow the prompts:
  - Provide an **admin email** and **FQDN hostname**.
  - Note the **admin URL** and **login credentials**.
  - Reboot the server when prompted.

---

## 2. Enabling phpMyAdmin SSO

Once HestiaCP is installed:

1. Log in to the HestiaCP control panel.
2. Navigate to:  
   `Server Settings > Databases`
3. Enable the toggle for **phpMyAdmin Single Sign On**.
4. Access any database directly via phpMyAdmin from the **List Databases** section.

---

## 3. Troubleshooting phpMyAdmin SSO

### 3.1 API Connection and IP Whitelist

- **API Status**:  
  Go to `Server Settings > Configure > Security > API` and ensure API access is **Enabled**.

- **Whitelist Public IP**:  
  Add your VPS's public IP to the **Allowed IPs** list.

- **Restart Services**:  
  Restart Apache/Nginx and the HestiaCP service:

```bash
sudo systemctl restart apache2
sudo systemctl restart nginx
sudo systemctl restart hestia
```

### 3.2 Security Token Mismatch

- **Toggle SSO**:  
  Disable and re-enable phpMyAdmin SSO under `Server Settings > Configure > Databases`.

- **Firewall/Proxy**:  
  Temporarily disable any firewall or proxy and retry the SSO login.

- **Link Expiration**:  
  Refresh the database page and click again if the link has expired.

---

## 4. Verifying HestiaCP API Access

To confirm API functionality:

1. Navigate to `Server Settings > Configure > Security > API`.
2. Ensure API access is **Enabled**.
3. Use curl or Postman to test API endpoints (e.g., list users, databases).
4. Confirm that responses are valid and authentication works.

---

## 5. SSO Functionality Test

To verify that phpMyAdmin SSO works across multiple databases:

1. Create at least **two or more databases** via HestiaCP.
2. Go to `List Databases` and click each database entry.
3. Confirm that each opens directly in phpMyAdmin without requiring manual login.

✅ If all databases open seamlessly, SSO is functioning correctly.

---

## 🛠️ Notes

- Always keep your HestiaCP instance updated for security patches.
- Avoid using browser extensions or proxies that may interfere with token-based redirects.
- For advanced debugging, inspect logs under `/var/log/hestia/` and `/var/log/nginx/`.

---

## 📬 Feedback & Contributions

Feel free to fork, improve, or suggest enhancements to this guide. Pull requests are welcome!
