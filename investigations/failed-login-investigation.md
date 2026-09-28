# Failed Login Investigation

## Summary
Five failed Windows network logon attempts were observed for the account `FakeSOCUser` on workstation `TENZIN992`.

## Evidence
- Event ID: 4625
- Account: FakeSOCUser
- Source IP: 127.0.0.1
- Workstation: TENZIN992
- Logon Type: 3
- Failure Reason: Unknown user name or bad password
- Failed Attempts: 5

## Analysis
The failed logon attempts originated from 127.0.0.1, which is the localhost address of the same Windows machine.

The attempts occurred within a short period of time and targeted the test account FakeSOCUser.

Because the activity was intentionally generated as part of the SOC lab, the behavior is expected and benign.

## Conclusion
Classification: Benign / Expected Test Activity

No escalation is required.