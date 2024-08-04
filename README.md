# Azure-SOC

# Building a SOC + Honeynet in Azure (Live Traffic)
![Azure-Cloud-Honeynet+SOC drawio](https://github.com/horacioxf/Azure-SOC/assets/100793672/b0580443-0258-4943-8cd8-e650fd7bc5fe)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics I will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into my honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Before Hardening_Security Controls drawio](https://github.com/horacioxf/Azure-SOC/assets/100793672/40d5f438-66a9-4766-b1d2-a9a630f22c64)


## Architecture After Hardening / Security Controls
![Architecture After Hardening_Security Controls drawio](https://github.com/horacioxf/Azure-SOC/assets/100793672/799f10bb-d498-49c9-b09e-e00369f065f6)

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
![Nsg-Malicious-Allowed-In](https://github.com/horacioxf/Azure-SOC/assets/100793672/e73557cd-d142-4d45-9152-5a59fb8feefe)<br>
![Linux-SSH-Auth_Fail](https://github.com/horacioxf/Azure-SOC/assets/100793672/769cd252-f9b6-4bd3-8a84-6c96cd1cf254)<br>
![MSSWL-Auth-Fail](https://github.com/horacioxf/Azure-SOC/assets/100793672/f5ae71b6-d540-42c7-8b8e-53edb3fdf4cb)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 24 hours:

| Start Time               | Stop Time
| ------------------------ | -----
|2024-06-13 16:32:41       | 2024-06-14 16:32:41

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 106637
| Syslog                   | 2099
| SecurityAlert            | 7
| SecurityIncident         | 243
| AzureNetworkAnalytics_CL | 1004

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics I measured in my environment for another 24 hours, but after I applied security controls:

| Start Time               | Stop Time
| ------------------------ | -----
|2024-06-17 22:25:58       | 2024-06-18 22:25:58


| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 17510
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Results

The following table shows an decrease in counts from the "BEFORE" and "AFTER" metrics.

| Metric                   | Change After Security Environment
| ------------------------ | -----
| SecurityEvent            | -83.58%
| Syslog                   | -99.95%
| SecurityAlert            | -100.00%
| SecurityIncident         | -100.00%
| AzureNetworkAnalytics_CL | -100.00%

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
