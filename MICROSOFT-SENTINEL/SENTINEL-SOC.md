# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/Flash028/Flash028/blob/6f356847b2ef7311d11ee75afdc86f29a89134ee/BUILDING%20A%20SOC/SOC.PNG)

## Introduction

In this project, I developed a minature honeynet in Azure and ingested log sources from various resources into a centralized Log Analytics workspace. This data is then leveraged by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. In this methodology I meticulously measured key security metrics within an initially vulnerable environment over a 24-hour period. Subsequently, I fortified the environment by implementing targeted security controls, after which I reassessed and measured the same metrics for an additional 24 hours. The following results encapsulate my findings:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)


## Architecture Before Hardening / Security Controls

  <p align="center">
  <img src="https://github.com/Flash028/Flash028/blob/7dc9e95ded7c68a77a4ecb431f2dc8e8fac65fb4/BUILDING%20A%20SOC/PUBLIC%20INTERNET.png" alt="Architecture Diagram">
</p>


## Architecture After Hardening / Security Controls

<p align="center">
  <img src="https://github.com/Flash028/Flash028/blob/94de8b2b17c525ea25ab830c6bd33452b9aeec4d/BUILDING%20A%20SOC/SUBNET.png" alt="Architecture Diagram">
</p>


The architecture of the miniature honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the Internet.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my administrative workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint.


## Attack Maps Before Hardening / Security Controls


![NSG Allowed Inbound Malicious Flows](https://github.com/Flash028/Flash028/blob/4db443c2100941ee3c6401d234eec5d8aed443b7/NSG.png)<br>
![Linux Syslog Auth Failures](https://github.com/Flash028/Flash028/blob/394f3288bd9926da7a87a83fc68b6226ce8e953e/SENTINEL%20MAPS/Linux.png)<br>
![Windows RDP/SMB Auth Failures](https://github.com/Flash028/Flash028/blob/0aa0f8c52a7821360ba0a3693be734eab6fb1ae9/SENTINEL%20MAPS/Windows.png)<br>


## Metrics Before Hardening / Security Controls



The following table shows the metrics measured in the insecure environment for 24 hours:
Start Time 2023-11-1T21:03:08.1360519Z
Stop Time 2023-11-21T21:03:08 1360519Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 33661
| Syslog                   | 1904
| SecurityAlert            | 6
| SecurityIncident         | 278
| AzureNetworkAnalytics_CL | 6243

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-11-23T22:46:31.74747427
Stop Time	2023-11-23T22:46:31.7474742Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9043
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a miniature honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

