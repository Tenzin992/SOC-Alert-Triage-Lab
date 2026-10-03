# Account Creation Investigation

## Summary

A new local Windows account was created on the lab system and detected through Windows Security Event ID 4720 in Splunk.

## Evidence

- Event ID: 4720
- Event: A user account was created
- Time: 10/02/2026 4:45:32.126 PM
- Host: LAB-PC
- Actor: LAB-PC\labuser
- New Account: SOC-TestUser

## Analysis

Windows Event ID 4720 records the creation of a new user account.

The Subject section identified the account that performed the action, while the New Account section identified the account that was created.

In this case, the account `LAB-PC\labuser` created the new local account `SOC-TestUser`.

Account creation is not automatically malicious. Administrators may create accounts for legitimate business, testing, or maintenance purposes.

The activity would become more suspicious if:

- the account was created outside normal business hours
- the actor was unexpected or unknown
- there was no approved change or ticket
- the new account was added to the Administrators group
- the account logged in shortly after creation
- other suspicious activity occurred around the same time

This activity was intentionally generated as part of the SOC lab.

## Conclusion

**Classification:** Benign / Expected Test Activity

The account creation was authorized and intentionally performed for testing purposes.

**Escalation:** Not required.