# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/user-attachments/assets/3d59ca6e-8499-4777-ac04-79eac6decf99)

## Introduction

In this project, I set up a mini honeynet in Azure, integrating various log sources into a Log Analytics workspace. This workspace is then utilized by Microsoft Sentinel to generate attack maps, trigger alerts, and create incidents. Initially, I measured security metrics in the unprotected environment over a 24-hour period. After implementing security controls to enhance the environment’s security, I conducted another 24-hour measurement. The metrics I'll show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/user-attachments/assets/a5c91d51-67bd-4f93-853d-6387a7732b29)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/user-attachments/assets/df81bd24-ac0b-4606-98e2-9f2c222e0d39)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were initially deployed and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls completely open, and all other resources were accessible via public endpoints, with no use of Private Endpoints.

For the "AFTER" metrics, Network Security Groups were fortified by blocking all traffic except from my admin workstation, and all other resources were secured using both their built-in firewalls and Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/user-attachments/assets/a0a32892-355e-4831-9154-8dfc2139f891)<br>
![Linux Syslog Auth Failures](https://github.com/user-attachments/assets/f19dec5d-5ed3-498e-891b-644bba544237)<br>
![Windows RDP/SMB Auth Failures](https://github.com/user-attachments/assets/56841a78-c214-42fe-a92d-946f61962cb5)<br>
![MS SQL Server Auth Failures](https://github.com/user-attachments/assets/cf9f5626-aa7e-4038-9d0b-be4ba093053b)<br>


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-08-07 20:02:22
Stop Time 2024-08-08 20:02:22

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 236861
| Syslog                   | 2107
| SecurityAlert            | 10
| SecurityIncident         | 216
| AzureNetworkAnalytics_CL | 1954

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-08-19 15:32:25
Stop Time	2024-08-20 15:32:25

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was built in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and generate incidents based on the collected logs. Security metrics were recorded both in the unsecured environment and again after implementing security controls. Notably, the number of security events and incidents significantly decreased, almost to zero, after the security measures were applied, highlighting their effectiveness.

It's important to mention that if the network resources were heavily used by regular users, there might have been a higher number of security events and alerts during the 24-hour period following the implementation of the security controls.
