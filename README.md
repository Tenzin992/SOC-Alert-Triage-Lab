# SOC Alert Triage & Investigation Lab

## Overview

This is a hands-on SOC project focused on investigating Windows security events using **Splunk**, **Windows Security Logs**, and **Sysmon**.

The goal is to practice the basic workflow of a SOC analyst:

1. Generate safe activity in a Windows lab environment
2. Collect Windows Security and Sysmon logs
3. Search and investigate activity in Splunk
4. Identify relevant evidence
5. Determine whether activity is benign or suspicious
6. Document findings and analyst conclusions

---

## Tools Used

- Splunk Enterprise
- Windows Event Viewer
- Microsoft Sysmon
- Windows Security Logs
- PowerShell
- Command Prompt

---

## Lab Workflow

```text
Windows Activity
        ↓
Windows Security / Sysmon Logs
        ↓
Splunk
        ↓
SPL Search
        ↓
Investigation
        ↓
Analyst Conclusion