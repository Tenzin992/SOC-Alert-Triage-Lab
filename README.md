\# SOC Alert Triage \& Investigation Lab



\## Overview



This is a hands-on beginner SOC project focused on investigating Windows security events using Splunk and Sysmon.



The goal of this project is to practice the basic workflow of a SOC analyst:



1\. Generate safe activity in a Windows lab environment

2\. Collect Windows Security and Sysmon logs

3\. Search and investigate the activity in Splunk

4\. Identify relevant evidence

5\. Decide whether the activity is benign or suspicious

6\. Document the investigation and conclusion



\## Tools



\- Splunk Enterprise

\- Windows Event Viewer

\- Microsoft Sysmon

\- Windows Security Logs

\- PowerShell

\- Command Prompt



\## Current Lab Setup



Windows Security and Sysmon logs are collected from a local Windows machine and ingested into Splunk for investigation.



Basic workflow:



Windows Activity  

→ Windows Security / Sysmon Logs  

→ Splunk  

→ SPL Search  

→ Investigation  

→ Analyst Conclusion



\## Investigations Completed



\### Failed Login Investigation



Generated controlled failed login attempts and investigated Windows Event ID 4625 in Splunk.



The investigation included:



\- Failed login count

\- Account name

\- Source address

\- Workstation

\- Logon type

\- Failure reason

\- Final analyst classification



\### PowerShell Investigation



Executed a harmless PowerShell command and investigated the corresponding Sysmon process-creation event.



The investigation included:



\- Process name

\- Command line

\- User

\- Process ID

\- Parent Process ID

\- Parent process

\- Process chain

\- Final analyst classification



\## Repository Structure



```text

SOC-Alert-Triage-Lab/

├── README.md

├── screenshots/

├── investigations/

├── splunk-searches/

└── incident-tickets/

