# Azure-Honeynet

![Azure-Honeynet-Arrows](https://github.com/gabriel-r100/Azure-Honeynet/assets/55646808/b7a7f4fb-47ab-4bb5-8a2b-4a5110d4b3a9)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which are then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

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
![2023-08-05 nsg-malicious-allowed-in BEFOREpng](https://github.com/gabriel-r100/Azure-Honeynet/assets/55646808/cc816686-fc23-4f9a-b5a0-9eb4c40473b4)<br>
![2023-08-05 syslog-ssh-auth-fail BEFORE](https://github.com/gabriel-r100/Azure-Honeynet/assets/55646808/3c4d6c00-1679-4b7c-a57b-6125dcbe0ed9)<br>
![2023-08-05 windows-rdp-smb-auth-fail BEFORE](https://github.com/gabriel-r100/Azure-Honeynet/assets/55646808/d7be805f-0bad-465d-9422-86111661cef3)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:<br>
Start Time 2023-08-05 19:57<br>
Stop Time 2023-08-06 19:57

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 95474
| Syslog                   | 3748
| SecurityAlert            | 4
| SecurityIncident         | 206
| AzureNetworkAnalytics_CL | 2482

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening. I triggered one alert on the RDP side to confirm the queries were still returning results.```

![2023-08-07 windows-rdp-smb-auth-fail AFTER](https://github.com/gabriel-r100/Azure-Honeynet/assets/55646808/2c52e2c0-bc30-4c65-acd4-e541d3d03ab2)

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:<br>
Start Time 2023-08-08 21:09<br>
Stop Time	2023-08-08 21:09

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10779
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

For my initial dive into cybersecurity, a simple honeynet was constructed within Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was utilized to trigger alerts and automatically generate incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. The drastic fall in security events attest to how efficient the controls implemented work in security an environment.

It is safe to assume that had users been utilizing the network, the likelihood of additional incidents increase as our security measures did not focus on user vulnerabilities.
