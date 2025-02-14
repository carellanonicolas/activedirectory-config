# Active Directory Setup in Azure

## Overview
This repository provides a structured approach to setting up an on-premises Active Directory environment within Microsoft Azure Virtual Machines.

## Prerequisites
Ensure the following are available before installation:
- Azure Virtual Machines (Windows Server 2022, Windows 10)
- Remote Desktop Access
- Active Directory Domain Services
- PowerShell for automation

## Installation Steps

### Part 1: Setting Up the Domain Controller
1. **Set up an Azure VM for DC:**
   - Create a Resource Group
   - Create a Virtual Network and Subnet
   - Deploy a Windows Server 2022 VM named `DC-1`
   - **Username:** labuser
   - **Password:** SamplePassword123!
   - Set the Private IP address of `DC-1` to static
   - Disable Windows Firewall for connectivity testing

2. **Create the Client VM:**
   - Deploy a Windows 10 VM named `Client-1`
   - **Username:** labuser
   - **Password:** SamplePassword123!
   - Attach it to the same Virtual Network as `DC-1`
   - Set `Client-1` DNS settings to `DC-1`'s Private IP
   - Restart `Client-1` from the Azure Portal
   - Log in to `Client-1` and verify connectivity to `DC-1`

3. **Install Active Directory on DC-1:**
   - Log into `DC-1`
   - Install Active Directory Domain Services (AD DS)
   - Promote `DC-1` to a domain controller for `mydomain.com`
   - Restart and log in as `mydomain.com\labuser`

### Part 2: Configuring Active Directory
1. **Create Organizational Units (OUs):**
   - In **Active Directory Users and Computers (ADUC)**:
     - Create an OU named `_EMPLOYEES`
     - Create an OU named `_ADMINS`
     - Create an OU named `_CLIENTS`

2. **Create Admin and User Accounts:**
   - In ADUC, create a new user:
     - **Name:** Jane Doe
     - **Username:** jane_admin
     - **Password:** SamplePassword123!
     - Add `jane_admin` to the `Domain Admins` Security Group
   - Log out and back in as `mydomain.com\jane_admin`

3. **Join Client-1 to the Domain:**
   - Log in as `labuser`
   - Join `Client-1` to `mydomain.com`
   - Restart `Client-1`
   - Verify `Client-1` appears in ADUC under `_CLIENTS`

### Part 3: Configuring Remote Desktop & User Management
1. **Enable Remote Desktop for Domain Users:**
   - Log into `Client-1` as `jane_admin`
   - Open System Properties → Remote Desktop
   - Allow **Domain Users** access to Remote Desktop
   - (In real-world settings, use Group Policy to configure multiple machines)

2. **Create Additional Users via PowerShell:**
   - Log in to `DC-1` as `jane_admin`
   - Open PowerShell ISE as Administrator
   - Run a script to generate multiple users in `_EMPLOYEES`
   - Verify user creation in ADUC
   - Log into `Client-1` as one of the new users
