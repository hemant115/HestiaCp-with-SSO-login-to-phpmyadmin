# HestiaCp-with-SSO-login-to-phpmyadmin

HestiaCP phpMyAdmin SSO and API access
This document provides a comprehensive guide to installing HestiaCP, troubleshooting phpMyAdmin Single Sign-On (SSO) issues, and verifying API access.
Table of contents
HestiaCP installation
Enabling phpMyAdmin SSO
Troubleshooting phpMyAdmin SSO
Verifying HestiaCP API access
1. HestiaCP installation
This section guides you through the initial setup of your HestiaCP instance.
1.1. Preparing the server
Use a fresh Debian or Ubuntu server (HestiaCP does not support RHEL-based systems).
Ensure root access and SSH connectivity.
Update and upgrade the system:
bash
sudo apt update && sudo apt upgrade -y
Use code with caution.

1.2. Downloading and running the installation script
Download the installation script:
bash
wget https://raw.githubusercontent.com/hestiacp/hestiacp/release/install/hst-install.sh
Use code with caution.

Run the script with superuser privileges, adding optional flags for desired components (e.g., Nginx, Apache, PHP-FPM, etc.):
bash
sudo bash hst-install.sh --apache yes --nginx yes --phpfpm yes --mysql yes --exim yes --dovecot yes --clamav yes --spamassassin yes --iptables yes --fail2ban yes
Use code with caution.

Follow the on-screen prompts, providing an admin email and an FQDN hostname.
Note down the admin URL and login credentials upon successful installation.
Reboot the server as instructed.
2. Enabling phpMyAdmin SSO
Once HestiaCP is installed, follow these steps to enable phpMyAdmin SSO:
Login to your HestiaCP control panel.
Navigate to Server Settings > Databases.
Locate and enable "phpMyAdmin Single Sign On".
Click on a database from the "List Databases" section to access it directly via phpMyAdmin.
3. Troubleshooting phpMyAdmin SSO
If phpMyAdmin SSO doesn't work after enabling it, follow these troubleshooting steps:
3.1. API connection and IP whitelist
API Status: Go to Server Settings > Configure > Security > API and confirm that API access is Enabled.
Whitelist Public IP: Add your VPS's public IP address to the allowed IPs in the HestiaCP server settings.
Restart Services: Consider restarting Apache/Nginx and the Hestia control panel service for changes to take effect.
3.2. Security token mismatch
Disable/Enable SSO: Go to Server Settings > Configure > Databases > phpMyAdmin Single Sign On, disable it, save, then re-enable it and save again.
Firewall/Proxy: Temporarily disable any firewalls or proxies and retry the SSO connection.
Link Expiration: If the link has expired, refresh the database page and try again.
3.3. File permissions and ownership
Incorrect File Group: If /etc/phpmyadmin has the wrong file group (e.g., hestiamail instead of www-data), use this command to correct it:
bash
sudo chown -R root:www-data /etc/phpmyadmin/
Use code with caution.

Other Permissions: Check and correct permissions for phpMyAdmin files:
bash
sudo chmod -R 755 /etc/phpmyadmin/
sudo chown -R hestiamail:www-data /usr/share/phpmyadmin/tmp/
Use code with caution.

3.4. Reconfiguring phpMyAdmin
Re-run Configuration: Try re-running the phpMyAdmin configuration:
bash
sudo dpkg-reconfigure phpmyadmin
Use code with caution.

Manual Installation Script: Consider re-running the phpMyAdmin installation script if available.
PMA User Password: If necessary, correct the pma user's password in the phpMyAdmin configuration file.
3.5. Apache and phpMyAdmin integration
Apache Configuration: If using Apache, ensure the line Include /etc/phpmyadmin/apache.conf exists in your Apache configuration file (e.g., /etc/apache2/apache2.conf).
Restart Apache: After any changes to Apache configuration, restart the service:
bash
sudo systemctl restart apache2
Use code with caution.

3.6. Review logs
HestiaCP Logs: Check logs in /var/log/hestia/* for errors related to phpMyAdmin SSO or API issues.
Apache/Nginx Logs: Examine Apache (e.g., /var/log/apache2/error.log) or Nginx logs for error messages related to phpMyAdmin or SSO.
4. Verifying HestiaCP API access
This section explains how to verify that the HestiaCP API is functioning correctly.
4.1. Access key and secret key authentication
Generate Access Key and Secret Key: Generate an Access Key and Secret Key within HestiaCP (or via the command line, as described in the documentation). The format will be <ACCESS_KEY>:<SECRET_KEY>.
Use cURL for testing:
bash
curl -k -X POST \
  --url https://your-server-ip:8083/api/ \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  --data 'hash=ACCESS_KEY:SECRET_KEY&cmd=v-list-web-domains&arg1=admin&returncode=yes'
Use code with caution.

Interpreting the Response:
A successful response will include a list of web domains and a return code of 0.
Error responses will have a non-zero return code and an error message (e.g., 10 for E_FORBIDEN, 15 for E_CONNECT).
4.2. HestiaCP API examples repository
Explore the HestiaCP API Examples repository for scripts and examples in various programming languages.
By following these instructions, you should be able to successfully install HestiaCP, enable phpMyAdmin SSO, and troubleshoot any issues that arise. If you continue to experience problems, consult the HestiaCP community forum for further assistance.
