# Scheduled Task Investigation

## Summary

A scheduled task was created and manually executed in a controlled Windows lab environment. The resulting process activity was investigated in Splunk using Sysmon process-creation logs.

## Evidence

- Task Name: SOC-Lab-Task
- Sysmon Event ID: 1
- Process: C:\Windows\System32\cmd.exe
- Command Line: cmd.exe /c echo SOC scheduled task ran > C:\Users\Public\soc-task-test.txt
- Parent Process: C:\Windows\System32\svchost.exe
- Parent Service: Windows Task Scheduler
- User: LAB-PC\labuser
- Result: C:\Users\Public\soc-task-test.txt was created successfully

## Analysis

The scheduled task launched `cmd.exe`, which executed a harmless command that wrote text to a local file.

The parent process was `svchost.exe`, and the parent command line showed the Windows Task Scheduler service. This indicates that the process was launched by Task Scheduler rather than manually from a normal Command Prompt window.

The activity was generated intentionally as part of the SOC lab.

Scheduled tasks can also be abused by attackers for persistence because they can automatically execute scripts or programs at specific times or system events.

The activity would become more suspicious if the task:

- launched an encoded or obfuscated PowerShell command
- executed a file from an unusual location such as AppData or Temp
- was created by an unexpected user
- ran repeatedly at unusual times
- downloaded files or contacted an unfamiliar external host
- launched an unknown executable or script
- had no approved business or administrative purpose

## Additional Observation

A search for Windows Security Event ID 4698 did not return results in the current lab environment.

However, the scheduled task was confirmed through Task Scheduler metadata and the associated Sysmon Event ID 1 process-creation events.

## Conclusion

**Classification:** Benign / Expected Test Activity

The scheduled task was intentionally created and executed for educational testing.

**Escalation:** Not required.