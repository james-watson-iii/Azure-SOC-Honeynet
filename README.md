# Building a SOC + Honeynet in Azure (Live Traffic)
![Project](https://github.com/user-attachments/assets/6a07722c-dd4b-42d2-b529-ae1f7ded92a7)


## Introduction

In this project, I built a mini honeynet in Azure and ingested log sources from various resources into a Log Analytics workspace, which was then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the unsecured environment for 24 hours, then applied security controls to harden the environment, measured metrics for another 24 hours, and have included the results below. The metrics that were measured include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Before](https://github.com/user-attachments/assets/70176b4d-991b-4772-a6c8-de772dfbb4e0)


## Architecture After Hardening / Security Controls
![After](https://github.com/user-attachments/assets/2bf9249d-dd84-4d1a-a271-e0d123157f51)


The architecture of the mini honeynet in Azure consisted of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls intentionally set wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint.

## Attack Maps Before Hardening / Security Controls
Allowed Network Security Group Malicious Flows
<img width="1152" alt="before-nsg-malicious-allowed-in" src="https://github.com/user-attachments/assets/bf069938-ca4d-4c8c-94d5-1dea87613e88">
<br>
<br>
Failed Linux SSH Login Attempts<img width="1205" alt="before-linux-ssh-auth-fail" src="https://github.com/user-attachments/assets/800aa7e9-120d-4a87-8e0f-67173cc259ad">
<br>
<br>
Failed Windows RDP Authorization Attempts
<br><img width="1082" alt="before-windows-rdp-auth-fail" src="https://github.com/user-attachments/assets/32405649-a8c8-4be0-8b90-dfe9c2eecb86">


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-08-25 21:15:15
Stop Time 2024-08-26 21:15:15

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10860
| Syslog                   | 21267
| SecurityAlert            | 2
| SecurityIncident         | 237
| AzureNetworkAnalytics_CL | 691

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-08-27 20:11:37
Stop Time	2024-08-28 20:11:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8559
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
