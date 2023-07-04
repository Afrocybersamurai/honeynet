# Building Honeynet in Azure + SOC: Cyber attacks in real time

![image](https://github.com/Afrocybersamurai/honeynet/assets/136266716/ae0ed791-6196-4592-a598-41e60eed02a3)

https://online.visual-paradigm.com/diagrams/features/azure-architecture-diagram-tool/
https://excalidraw.com/

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Azure components utilised:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel


## Architecture Before Hardening / Security Controls
![image](https://github.com/Afrocybersamurai/honeynet/assets/136266716/d1581a98-9c71-4231-851a-cd3ada79922d)



## Architecture After Hardening / Security Controls
![image](https://github.com/Afrocybersamurai/honeynet/assets/136266716/a2966eac-e767-4a91-ac02-ac6fdb49a70f)





For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Utilizing NIST 800.61r2 Computer Incident Handling Guide

For each simulated attack I practiced incident responses following NIST SP 800-61 r2.

![NIST 800.61](https://i.imgur.com/6PTG7c0l.png)

Each organization will have policies related to an incident response that should be followed. This event is just a walkthrough for possible actions to take in the detection of malware on a workstation.  

#### Preparation

- The Azure lab was set up to ingest all of the logs into Log Analytics Workspace, Sentinel and Defender were configured, and alert rules were put in place.

#### Detection & Analysis

- Malware has been detected on a workstation with the potential to compromise the confidentiality, integrity, or availability of the system and data.
- Assigned alert to an owner, set the severity to "High", and the status to "Active"
- Identified the primary user account of the system and all systems affected.
- A full scan of the system was conducted using up-to-date antivirus software to identify the malware.
- Verified the authenticity of the alert as a "True Positive".
- Sent notifications to appropriate personnel as required by the organization's communication policies.

#### Containment, Eradication & Recovery

- The infected system and any additional systems infected by the malware were quarantined.
- If the malware was unable to be removed or the system sustained damage, the system would have been shut down and disconnected from the network.
- Depending on organizational policies the affected systems could be restored known clean state, such as a system image or a clean installation of the operating system and applications. Or an up-to-date anti-virus solution could be used to clean the systems. 

#### Post-Incident Activity

- In this simulated case, an employee had downloaded a game that contained malware. 
- All information was gathered and analyzed to determine the root cause, extent of damage, and effectiveness of the response. 
- Report disseminated to all stakeholders.
- Corrective actions are implemented to remediate the root cause.
- And a lessons-learned review of the incident was conducted.

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
