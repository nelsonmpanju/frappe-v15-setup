# frappe-v15-setup

## Introduction

This document provides a comprehensive, step-by-step guide to setting up the environment and installing frappe  Version 15 on a newly installed Ubuntu 22.04 operating system. Following these instructions will ensure optimal functionality and performance of Frappe Apps (eg. ERPNext ,  HRMS ) on your server.

## Prerequisites

Before proceeding with the installation, ensure that your system meets the following software and hardware requirements.

### **Software Requirements**

* **Ubuntu 22.04** : A fresh and fully updated installation.
* **Sudo Privileges** : Access to a user account with `sudo` permissions.
* **Python 3.10 or higher** : Required for running ERPNext.
* **Node.js 18** : Necessary for frontend dependencies and build processes.

### **Hardware Requirements**

* **Memory (RAM)** : Minimum of **4GB** RAM for smooth operation.
* **Storage** : At least **40GB** of free hard disk space.

## **Server Settings**

Before we begin installing Frappe, let's perform some initial server configurations to ensure a smooth installation process.

### **Update and Upgrade Packages**

First, update the package list and upgrade existing packages to their latest versions:

```bash
sudo apt update -y
sudo apt upgrade -y
```

### **Create a New User for Frappe Bench**

It's best practice not to use the root user for everyday tasks due to security reasons. We'll create a new user that will own the Frappe Bench processes.

**Create a New User** (Replace `frappe-user` with your desired username):

```bash
sudo adduser frappe-user
```

* You'll be prompted to set a password and provide user information. Fill in the details or press `Enter` to skip.

**Add User to Sudo Group** :

```bash
sudo usermod -aG sudo frappe-user
```

* This grants the new user administrative privileges.

 **Switch to the New User** :

```bash
su - frappe-user
```

 **Navigate to Home Directory** :

```bash
cd /home/frappe-user
```

**Note** : Ensure you replace `frappe-user` with your chosen username throughout the commands.

## **Install Required Packages**

To ensure Frappe runs smoothly, we need to install several essential packages and dependencies on our Ubuntu 22.04 system.

### **Install Git**

Git is a version control system that allows us to clone repositories and manage code versions.

```bash
sudo apt install git -y
```

### **Install Python 3.10 and Development Packages**

Frappe Version 15 requires Python 3.10 or higher. We'll install Python and necessary development packages

```bash
sudo apt install python3-dev python3.10-dev python3-setuptools python3-pip python3-distutils -y
```

### **Install Python Virtual Environment**

A virtual environment helps manage dependencies for our Python projects in isolation.

```bash
sudo apt install python3.10-venv -y
```

### **Install Software Properties Common**

This package provides scripts for managing software repositories in Debian-based systems.

```bash
sudo apt install software-properties-common -y
```

### **Install MariaDB**

MariaDB is the default database system used by Frappe.

```bash
sudo apt install mariadb-server mariadb-client -y
```

### **Install Redis Server**

Redis is an in-memory data structure store used by Frappe  for caching.

```bash
sudo apt install redis-server -y
```

### **Install Additional Packages**

Frappe relies on additional packages for rendering PDFs, handling fonts, and other functionalities.

```bash
sudo apt install xvfb libfontconfig wkhtmltopdf libmysqlclient-dev -y
```

## **Configure MariaDB**

We need to secure and configure MariaDB to work properly with Frappe.

### **Secure MariaDB Installation**

Run the following command to secure your MariaDB installation:

```bash
sudo mysql_secure_installation
```

When prompted, follow these recommended responses:

1. **Enter current password for root** : *(Press Enter if no password is set)*
2. **Switch to unix_socket authentication [Y/n]** : **Y**
3. **Change the root password? [Y/n]** : **Y**
   * **New password** : *Enter a strong password for the MariaDB root user*
4. **Remove anonymous users? [Y/n]** : **Y**
5. **Disallow root login remotely? [Y/n]** : **N**
   * *We choose 'N' to allow remote access if needed for tools like Metabase or PowerBI.*
6. **Remove test database and access to it? [Y/n]** : **Y**
7. **Reload privilege tables now? [Y/n]** : **Y**

### **Configure MariaDB Character Set**

Edit the MariaDB configuration file to set the correct character set.

**Open the configuration file** :

```bash
sudo nano /etc/mysql/my.cnf
```

 **Add the following configuration** :

```sql
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

[mysql]
default-character-set = utf8mb4
```

**Save and exit** :

* Press `Ctrl + O` to save.
* Press `Enter` to confirm the filename.
* Press `Ctrl + X` to exit the editor

### **Restart MariaDB Service**

Apply the changes by restarting MariaDB:

```bash
sudo service mysql restart
```

## **Install cURL, Node.js, npm, and Yarn**

These tools are required for installing Node.js packages and managing frontend assets in Frappe.

### **Install cURL**

cURL is used to transfer data with URLs, which we'll use to install Node Version Manager (nvm).

```bash
sudo apt install curl -y
```

### **Install Node Version Manager (nvm)**

nvm allows us to manage multiple versions of Node.js.

* **Download and install nvm** :

```bash
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
```

**Activate nvm** :

```bash
source ~/.profile
```

### **Install Node.js Version 18**

Use nvm to install Node.js version 18, which is required by Frappe version 15.

```bash
nvm install 18
```

**Verify the installation** :

```bash
node -v
npm -v
```

### **Install Yarn**

Yarn is a package manager that handles project dependencies efficiently.

```bash
npm install -g yarn
```

**Verify the installation** :

```bash
yarn --version
```

## **Install Frappe Bench**

Now that we have all the necessary dependencies installed, we can proceed to install Frappe Bench, which is the command-line tool used to manage Frappe and its applications.

### **Install Frappe Bench Using pip**

We'll use `pip3` to install the Frappe Bench CLI globally.

```bash
sudo pip3 install frappe-bench
```

## **Initialize Frappe Bench**

Next, we'll initialize a new bench directory. This will set up the required directory structure and download the Frappe framework

```bash
bench init --frappe-branch version-15 frappe-bench
```

* This command initializes a bench named `frappe-bench` with Frappe Version 15.

## **Navigate to the Bench Directory**

Change your current directory to the newly created bench directory.

```bash
cd frappe-bench
```

## **Adjust User Directory Permissions**

We need to ensure that the bench user has the necessary permissions to execute files within their home directory.

```bash
sudo chown -R frappe-user:frappe-user /home/frappe-user
```

* Replace `frappe-user` with the username you created earlier (e.g., `frappe`).
* This command grants read and execute permissions to others for the bench user's home directory and its contents

## **Create a New Site**

In Frappe, a "site" represents an instance where the applications run. We need to create a new site to install Frappe apps (e.g., ERPNext, HRMS)

```bash
bench new-site site-name
```

* Replace `site-name` with your desired site name (e.g., `mysite.local`).
* You'll also need to set an administrator password for the new site when prompted.

## **Install ERPNext and Other Applications**

Before proceeding, it's important to note the dependencies between the applications:

* **HRMS** depends on  **ERPNext** .
* **ERPNext** depends on the **Payments** app.

Therefore, we need to install them in the correct order to ensure all dependencies are satisfied.

### **Download the Required Applications**

We'll use the `bench get-app` command to download the applications we need.

#### **1. Install the Payments App**

The Payments app is required for setting up ERPNext.

```bash
bench get-app --branch version-15 payments
```

#### **2. Install ERPNext**

Next, we'll download ERPNext, specifying the version 15.

```bash
bench get-app --branch version-15 erpnext
```

#### **3. Install the HRMS App**

Now, we'll download the HRMS application.

```bash
bench get-app --branch version-15 hrms
```

### **Install the Applications on Your Site**

After downloading the applications, we need to install them on our site in the correct order.

#### **1. Install Payments on the Site**

```bash
bench --site site-name install-app payments
```

* Replace `site-name` with the name of your site (e.g., `mysite.local`).

#### **2. Install ERPNext on the Site**

```bash
bench --site site-name install-app erpnext
```

* Replace `site-name` with the name of your site (e.g., `mysite.local`).

#### **3. Install the HRMS App on the Site**

```bash
bench --site site-name install-app hrms
```

### **Start the Bench**

With the applications installed, we can start the bench to run our ERPNext instance.

```bash
bench start
```

**Note** : This command starts the development server, which will run until you stop it. If you close the terminal or the server restarts, you'll need to run `bench start` again.
