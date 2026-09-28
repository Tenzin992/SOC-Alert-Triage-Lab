# PowerShell Investigation

## Summary

A PowerShell process was observed on the Windows host as part of a controlled SOC lab exercise.

## Evidence

- Sysmon Event ID: 1
- Process: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
- Command Line: powershell.exe -NoProfile -Command "Get-Date"
- User: Tenzin992\tenzin
- Parent Process ID: 21496
- Parent Process: C:\Windows\System32\cmd.exe
- Parent of cmd.exe: C:\Windows\explorer.exe
- Sysmon UTC Time: 2026-09-28 16:39:31.058

## Analysis

Sysmon Event ID 1 shows that a PowerShell process was created.

The command line shows that PowerShell executed the harmless Get-Date command.

The original PowerShell event did not contain the parent process image, but it provided ParentProcessId 21496. Searching Sysmon process-creation events for ProcessId 21496 identified the parent process as cmd.exe.

The process chain was:

explorer.exe -> cmd.exe -> powershell.exe

This matches the expected activity because Command Prompt was manually opened and used to launch PowerShell.

PowerShell can also be abused by attackers because it can execute scripts, download content, interact with the operating system, and launch other processes. Additional evidence such as encoded commands, suspicious downloads, unusual parent processes, unexpected network connections, or unknown user activity would increase suspicion.

## Conclusion

Classification: Benign / Expected Test Activity

The PowerShell command was intentionally executed as part of the SOC lab and performed only a harmless Get-Date operation.

No escalation is required.