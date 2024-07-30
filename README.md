# Building a SOC + Honeynet in Azure (Live Traffic)
<br>![Honeynet+SOC Environment Setup](https://github.com/user-attachments/assets/1a4309cc-a67f-4c8f-91d7-3496cbc76e02)


## Introduction

I built a small-scale honeynet in Microsoft Azure, an isolated network designed to lure potential attackers and gather information about their activities. I collected log data from various sources and directed it to a Log Analytics workspace. Microsoft Sentinel, a cloud-native SIEM (Security Information and Event Management) solution, utilized this data to create visual attack maps, trigger alerts upon detecting suspicious activities, and generate detailed incident reports for investigation. Initially, I gathered security metrics in an unprotected environment over 24 hours. I implemented a series of security controls to strengthen the environment. I then measured the security metrics for an additional 24 hours to evaluate the impact of these measures. The resulting data is displayed below. The security metrics we will present include: 

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

Before the changes, all resources were initially deployed in a manner that made them accessible from the internet. Specifically, the Network Security Groups and built-in firewalls of the Virtual Machines were configured to allow all traffic, and all other resources had public endpoints that were visible to the internet, rendering Private Endpoints unnecessary.

After the changes, the Network Security Groups were strengthened by implementing a policy to block all traffic, with the exception of access from the admin workstation. Additionally, all other resources were safeguarded by their built-in firewalls and by utilizing Private Endpoints for added security.
## Attack Maps Before Hardening / Security Controls
![Last 24 hrs NSG allowed in US](https://github.com/user-attachments/assets/a8b5b95a-7743-4c19-8154-64db60d7a4b8) <br>
![Last 24hrs Linux US](https://github.com/user-attachments/assets/16977489-47b2-4982-b1aa-3c312f0131e8)<br>
![last 24 hrs Windows RDP fail US](https://github.com/user-attachments/assets/b61b9592-0def-4819-86fb-77d565215f25)<br>
![Last 24hrs SQL US](https://github.com/user-attachments/assets/e3b57b0e-5a5d-4db1-8d3d-141a5995b2fe)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 24 hours:
Start Time 2024-07-26 11:23 PM
Stop Time 2024-07-27 11:27 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 53098
| Syslog                   | 20743
| SecurityAlert            | 0
| SecurityIncident         | 146
| AzureNetworkAnalytics_CL | 3860

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

Security Events (Windows VMs)	-83.77%
Syslog (Linux VMs)	-99.87%
SecurityAlert (Microsoft Defender for Cloud)	#DIV/0!
Security Incident (Sentinel Incidents)	-100.00%
NSG Inbound Malicious Flows Allowed	-100.00%


```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Conclusion

As part of this project, I set up a mini honeynet in Microsoft Azure and integrated various log sources into a Log Analytics workspace. I utilized Microsoft Sentinel to trigger alerts and create incidents based on the gathered logs to detect and respond to security threats. I conducted a thorough analysis of metrics in the unprotected environment before implementing security measures. Afterward, I re-evaluated these metrics once the security controls were in place. The results showed a significant reduction in the number of security events and incidents, demonstrating the effectiveness of the security measures.

It's important to note that if regular users had heavily utilized the network resources, there might have been a higher volume of security events and alerts generated within the 24-hour period following the implementation of the security controls.
