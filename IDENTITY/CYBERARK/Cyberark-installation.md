# CyberArk Installation and Configuration Guide

This guide provides detailed installation and configuration steps for setting up a CyberArk environment with the following components:

- **Active Directory (AD)** - IP: `192.168.100.10`
- **Vault** - IP: `192.168.100.11`
- **CPM (Central Policy Manager)** - IP: `192.168.100.12`
- **PSM (Privileged Session Manager)** - IP: `192.168.100.13`

## Installation Steps

### 1. Set Up IP Configuration

Configure IP addresses for the following components:

- **AD**: `192.168.100.10`
- **Vault**: `192.168.100.11`
- **CPM**: `192.168.100.12`
- **PSM**: `192.168.100.13`

Ensure that all servers are able to communicate with each other and that DNS and firewall settings allow proper connectivity.

### 2. Install CyberArk Components

#### Install Vault Component

Navigate to the **Vault** server (`192.168.100.11`) and run the installer for the Vault component. Follow the installation wizard prompts to install the Vault. Choose to install the Vault with the necessary options (typically including the Vault Database, Vault Service, and the Vault Web Client). Once installed, the Vault will automatically start the services.

#### Install CPM Component

Navigate to the **CPM** server (`192.168.100.12`) and run the installer for the Central Policy Manager (CPM) component. Follow the installation wizard prompts to install the CPM. Configure the CPM to connect to the Vault and perform necessary operations like password management and policy enforcement.

#### Install PSM Component

Navigate to the **PSM** server (`192.168.100.13`) and run the installer for the Privileged Session Manager (PSM) component. Follow the installation wizard prompts to install the PSM. The PSM will be configured to connect to the Vault and allow session management for privileged access.

### 3. Configuration of CyberArk Components

After installing each component, proceed with the following configuration:

#### Configure Vault

Access the Vault via the CyberArk Web Console. Configure the Vault to store privileged account credentials. Set up the required users and policies for account management. Configure Vault backup and disaster recovery options.

#### Configure CPM

In the CPM component, configure policies to define how privileged accounts should be managed (e.g., rotation policies, password policies). Ensure CPM can communicate with the Vault to retrieve and store passwords. Set up any necessary notification or reporting for policy enforcement.

#### Configure PSM

In the PSM component, configure session monitoring and recording for privileged users. Ensure PSM is connected to the Vault to allow session management. Set up auditing and reporting for privileged sessions.

### 4. PVWA Web Console Access

After installation and configuration, navigate to the PVWA Web Console to begin working with the CyberArk solution. Open a browser and go to: `https://192.168.100.11/PVWA`. Log in with the appropriate administrator credentials. Access the PVWA console to manage accounts, sessions, and policies.

## Notes

- Ensure that your firewall settings are properly configured to allow communication between the components.
- If you encounter issues, refer to the CyberArk documentation or check the logs on each component.
- Each CyberArk component (Vault, CPM, PSM) should be installed and configured independently, with appropriate settings for your network and security requirements.

## Troubleshooting

If the installation does not proceed as expected, check the following:

- Ensure all prerequisites are installed.
- Verify network connectivity between all servers (AD, Vault, CPM, and PSM).
- Check installation logs for any errors:
  - Vault logs: `C:\Program Files\CyberArk\Vault\Log`
  - CPM logs: `C:\Program Files\CyberArk\CPM\Log`
  - PSM logs: `C:\Program Files\CyberArk\PSM\Log`



   
