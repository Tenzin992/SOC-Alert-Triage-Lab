# Splunk Detection Searches

This file contains repeatable Splunk searches created from the investigation scenarios completed in the SOC Alert Triage & Investigation Lab.

---

## Detection 1 — Repeated Failed Logins

### SPL

```spl
index=windows EventCode=4625
| eval FailedAccount=mvindex(Account_Name,-1)
| bin _time span=5m
| stats count as FailedAttempts by _time FailedAccount
| where FailedAttempts >= 5
| sort - _time
```

### What It Detects

This search identifies accounts with at least five failed Windows logon attempts within a five-minute period.

Repeated failed logins in a short period may indicate password guessing or brute-force activity.

### Likely False Positive

A legitimate user repeatedly entering the wrong password could trigger this detection.

### Tuning Note

The `Account_Name` field contained multiple values in the lab data.

The following line was used to isolate the failed account:

```spl
| eval FailedAccount=mvindex(Account_Name,-1)
```

The five-minute time window prevents failed logins from different days from being counted together.

---

## Detection 2 — PowerShell Process Activity

### SPL

```spl
index=windows source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1
Image="*powershell.exe"
NOT Image="*splunk-powershell.exe"
| table _time User Image CommandLine ParentImage ParentCommandLine ProcessId ParentProcessId
```

### What It Detects

This search identifies PowerShell process-creation events recorded by Sysmon Event ID 1.

The results show:

- Time
- User
- Process path
- Command line
- Parent process
- Parent command line
- Process ID
- Parent Process ID

These fields help an analyst understand how PowerShell was launched and what command it executed.

### Likely False Positive

Legitimate administrators, installers, IT tools, automation scripts, and normal software may use PowerShell.

PowerShell activity by itself does not mean malicious activity occurred.

### Tuning Note

The original search returned thousands of events because Splunk frequently used:

```text
splunk-powershell.exe
```

The search was tuned with:

```spl
NOT Image="*splunk-powershell.exe"
```

to reduce known Splunk-related noise.

---

## Detection 3 — Windows Account Creation

### SPL

```spl
index=windows EventCode=4720
| eval ActorAccount=mvindex(Account_Name,0)
| eval NewAccount=mvindex(Account_Name,-1)
| table _time ComputerName ActorAccount NewAccount
```

### What It Detects

This search identifies Windows Event ID 4720 events, which indicate that a user account was created.

The search separates the account information into:

- `ActorAccount` — the account that created the new account
- `NewAccount` — the account that was created

For example:

```text
ActorAccount: tenzin
NewAccount: SOC-TestUser
```

means that the `tenzin` account created the new `SOC-TestUser` account.

### Likely False Positive

A legitimate administrator may create a new employee, service, maintenance, or test account.

### Analyst Note

Account creation is not automatically malicious.

An analyst should verify whether the account creation was expected and authorized and investigate additional activity such as:

- the new account being added to an administrative group
- unexpected logins by the new account
- account creation outside normal business hours
- activity performed by an unusual or unauthorized actor

---

## Summary

These searches convert individual investigations into repeatable detection logic.

The main lessons from creating these detections were:

- Detection searches often require tuning.
- Field extraction should be verified before relying on a field.
- Time windows matter when detecting repeated activity.
- Legitimate activity can trigger security detections.
- Analysts must review context before deciding whether activity is malicious.