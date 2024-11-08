# Centreon-

# Centreon Installation Guide

Centreon provides RPM and DEB packages for monitoring solutions, available for free through the Centreon Open Source version. This guide walks you through the installation process on supported Linux distributions,here is the entire guide for Debian 12.

## Prerequisites

1. **Update the Server**  
   After setting up your server, update the operating system by running:
   
   ```bash
   apt update && apt upgrade
   ```
   - Accept all GPG keys and reboot the server if prompted for a kernel update.

3. **Pre-installation Requirements**  
   - **Disable SELinux**: Not installed on Debian 12, but ensure it’s disabled on Alma/RHEL/Oracle Linux.
   - **Firewall**: Configure firewall rules if active, or disable it temporarily during installation:
     ```bash
     systemctl stop firewalld
     systemctl disable firewalld
     ```

## Step 1: Install Repositories and Dependencies

1. **Install Dependencies**  
   ```bash
   apt update && apt install lsb-release ca-certificates apt-transport-https software-properties-common wget gnupg2 curl
   ```

2. **Add Sury APT Repository for PHP 8.2**  
   ```bash
   echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/sury-php.list
   wget -O- https://packages.sury.org/php/apt.gpg | gpg --dearmor | tee /etc/apt/trusted.gpg.d/php.gpg > /dev/null 2>&1
   apt update
   ```

3. **Set Up Database Repository**  
   - For **MariaDB** (Debian 12):
     ```bash
     curl -LsS https://r.mariadb.com/downloads/mariadb_repo_setup | sudo bash -s -- --os-type=debian --os-version=12 --mariadb-server-version="mariadb-10.11"
     ```

4. **Add Centreon Repository**  
   ```bash
   echo "deb https://packages.centreon.com/apt-standard-24.10-stable/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/centreon.list
   echo "deb https://packages.centreon.com/apt-plugins-stable/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/centreon-plugins.list
   wget -O- https://apt-key.centreon.com | gpg --dearmor | tee /etc/apt/trusted.gpg.d/centreon.gpg > /dev/null 2>&1
   apt update
   ```

## Step 2: Install Centreon

To set up a Centreon central server, either with a local or remote database:

1. **Install Centreon Packages**  
   ```bash
   apt update
   apt install -y centreon-mariadb centreon
   systemctl daemon-reload
   systemctl restart mariadb
   ```

## Step 3: Configure Centreon

1. **Set the Server Name**  
   Replace `new-server-name` with your desired hostname:
   ```bash
   hostnamectl set-hostname new-server-name
   ```

2. **Enable Services on Boot**  
   Enable required services to start on boot:
   ```bash
   systemctl enable php8.2-fpm apache2 centreon cbd centengine gorgoned centreontrapd snmpd snmptrapd
   ```

3. **Enable Database Service**  
   If using a local database (or on the remote database server):
   ```bash
   systemctl enable mariadb
   systemctl restart mariadb
   ```

4. **Secure the Database**  
   Secure root access to the database before finalizing the Centreon setup.

---

Follow this README for a successful Centreon installation and initial configuration. For more detailed options and troubleshooting, visit [Centreon's official documentation](https://docs.centreon.com/).
