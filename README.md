# Building a SOC + Honeynet in Azure (Live Traffic)
[Honeynet+SOC Environment Setup](https://github.com/user-attachments/assets/0423811a-8bf9-4122-9172-cded4ed57123)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![2024-07-30 (2)](https://github.com/user-attachments/assets/82f84dbf-c747-4f4c-9c3f-09baaf71794a)


## Architecture After Hardening / Security Controls
![2024-07-30 (2)](https://github.com/user-attachments/assets/8646635d-f10d-4aac-8186-7fc69a4ec698)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![Last 24 hrs NSG allowed in US](https://github.com/user-attachments/assets/a8b5b95a-7743-4c19-8154-64db60d7a4b8) <br>
![Last 24hrs Linux US](https://github.com/user-attachments/assets/16977489-47b2-4982-b1aa-3c312f0131e8)<br>
![last 24 hrs Windows RDP fail US](https://github.com/user-attachments/assets/b61b9592-0def-4819-86fb-77d565215f25)<br>
![Last 24hrs SQL US](https://github.com/user-attachments/assets/e3b57b0e-5a5d-4db1-8d3d-141a5995b2fe)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-07-26 11:23 PM
Stop Time 2024-07-27 11:27 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 53098
| Syslog                   | 20743
| SecurityAlert            | 0
| SecurityIncident         | 146
| AzureNetworkAnalytics_CL | 3860

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-07-29 4:00 AM
Stop Time	2024-07-30 4:08

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8620
| Syslog                   | 27
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
