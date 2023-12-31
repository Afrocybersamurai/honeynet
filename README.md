# Building Honeynet in Azure + SOC: Cyber attacks in real time

![image](https://user-images.githubusercontent.com/136266716/260706919-b2f370a4-de20-444e-9014-3fe3a64eebfa.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. 

I measured some security metrics in the insecure environment for 24 hours, 
applied some security controls to harden the environment, 
amd then remeasured metrics for another 24 hours. 
These results are detailed below. 

The metrics we will measure were:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)



## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)


## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)



## Azure components utilised:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel





## Attack Maps Before Hardening / Security Controls

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

![Linux SSH Auth Failure (Before)](https://user-images.githubusercontent.com/109401839/235823253-36926afe-3fbb-4292-a47b-69b3224f48fb.png)

![MySQL Authentication Failures(Before)](https://user-images.githubusercontent.com/109401839/235823254-a3fb0c78-d5b0-4bf2-81b1-6ed1b85dead3.png)

![nsg-malicious-allowed-in (before)](https://user-images.githubusercontent.com/109401839/235823255-188f63c4-9d83-445b-9400-ca44041e7f2c.png)

![Windows RDP   SMB Authentication Failure(Before)](https://user-images.githubusercontent.com/109401839/235823259-ba8c71e4-3a75-4120-849e-7d9292b410a7.png)

## Metrics Before Hardening / Security Controls
For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29


| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 39046
| Syslog                   | 782
| SecurityAlert            | 1
| SecurityIncident         | 222
| AzureNetworkAnalytics_CL | 1350


```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0 (-100%)
| Syslog                   | 0 (-100%)
| SecurityAlert            | 0 (-100%) 
| SecurityIncident         | 0 (-100%)
| AzureNetworkAnalytics_CL | 0 (-100%)

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
