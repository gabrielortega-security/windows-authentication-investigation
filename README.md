# Windows Authentication Investigation

## Overview
This project investigates normal versus suspicious Windows logon activity on a Windows 11 system. Multiple failed logon attempts were generated for a local account, followed by successful authentication. The goal was to practice collecting evidence from Windows Event Logs, building a timeline, analyzing the activity, and mapping it to the MITRE ATT&CK framework.

## Lab Environment
- Hypervisor: VMware
- Operating System: Windows 11
- Tools Used: Windows Event Viewer
- Computer Name: WIN11-SOC

## Objectives
- Generate and capture normal and suspicious logon activity
- Identify key Windows Security Event IDs (4624 and 4625)
- Build a clear timeline of events
- Analyze the activity and map it to MITRE ATT&CK
- Practice professional documentation of an investigation

## Activity Performed
Normal successful logons were first generated. Then multiple failed logon attempts were performed against the local account “gabriel” by intentionally entering incorrect passwords. After the failed attempts, a successful logon was completed with the correct password.

## Timeline of Events
- 6:21:56 PM – Failed logon attempt for account “gabriel” (Event ID 4625)
- 6:22:06 PM – Failed logon attempt for account “gabriel” (Event ID 4625)
- 6:22:34 PM – Failed logon attempt for account “gabriel” (Event ID 4625)
- 6:26:07 PM – Successful logon for account “gabriel” (Event ID 4624)

All observed logon attempts (failed and successful) were Logon Type 2 (Interactive).

## Key Evidence

### Failed Logons (Event ID 4625)
![Failed Logon 1](screenshots/Screenshot%202026-08-18%20183559.png)
![Failed Logon 2](screenshots/Screenshot%202026-08-18%20183740.png)
![Failed Logon 3](screenshots/Screenshot%202026-08-18%20183822.png)

### Successful Logon (Event ID 4624)
![Successful Logon 1](screenshots/Screenshot%202026-08-18%20184544.png)

## Analysis
Between 6:21 PM and 6:22 PM, multiple failed logon attempts occurred for the local account “gabriel”. The failure reason recorded was “Unknown user name or bad password.” A few minutes later, at 6:26 PM, a successful interactive logon (Logon Type 2) was recorded for the same account.

This pattern is consistent with password guessing or a low-and-slow brute-force attempt followed by successful authentication. In a real environment, this activity would warrant further investigation, including reviewing the source of the attempts and confirming whether account lockout policies were effective.

## MITRE ATT&CK Mapping
- **T1110.001 – Password Guessing**  
  Multiple failed logon attempts using incorrect passwords against the same account.

- **T1078 – Valid Accounts**  
  Successful logon using a legitimate account after the failed attempts.

## Findings & Recommendations
- Multiple failed logons followed by a successful logon can indicate password guessing.
- Interactive (Type 2) logons from the console are expected for local access but should still be monitored when combined with failures.
- Recommendations in a real environment would include:
  - Ensuring account lockout policies are properly configured
  - Monitoring for repeated 4625 events
  - Reviewing privileged account usage
  - Considering multi-factor authentication where possible

## Lessons Learned
- Windows Event Viewer is a valuable native tool for investigating authentication activity.
- Event IDs 4624 and 4625 provide clear evidence of successful and failed logons.
- Building a simple timeline makes patterns much easier to recognize.
- Mapping activity to MITRE ATT&CK helps communicate findings in a standardized way.
- Proper documentation turns raw log data into a professional investigation artifact.
