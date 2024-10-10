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
