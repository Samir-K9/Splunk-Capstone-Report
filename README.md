# Splunk-Capstone-Report
## Scenario:
Ryan Adams, a local administrator at a small dental clinic, contacted the MahCyberDefense SOC hotline after suspecting his computer may have been compromised. He
explained that around October 15 2025 at 13:00 UTC, his mouse was randomly moving so he became suspicious.
As a SOC analyst, your task is to investigate this incident using Splunk. Analyze the available data to determine what occurred, identify any indicators of compromise, and document your findings in a SOC report using the provided format found in the community.

## Findings:
- Time: 2025-10-25 12:51:44 UTC 
- Host: FRONTDESK-PC1
- Host IP: 172.16.0.110
- User: Ryan.Adams
- IOC IP: 172.16.0.184
- IOC C2 IP: 157.245.46.190
- Filename: python.exe
- SHA256 Hash:  CFFAB896E9F0B12101034D9CED76332EF5AA4036AFA08E940E825E277C21A044

## Investigation Summary
On 2025-10-25 12:51:44 UTC, a series of password attempts targetted several accounts on the workstation `FRONTDESK-PC1`.  

These attempts were made from the IP address of `172.16.0.184` from `DESKTOP-924H12`.  

The accounts that were targetted include `administrator` , `guest` , `andrew.henderson` and `ryan.adams`.  

As different accounts were being targetted, this shows that attacker was attempting Password Spraying attack.  

The user was able to successfully login using the credentials of `Ryan.Adams`.

Upon further investigation, it was identified that no other accounts have been compromised.

After gaining access, the next move from the attacker was disabling Windows Defender antivirus and turning off Windows Defender nottifications.

An executable called `python.exe` was downloaded from Google Chrome and was saved to the directory `C:\Users\Ryan.Adams\Music\python.exe`. 

This file was executed using Windows Explorer which initiated an outbound TCP connection to `157.245.46.190:8888` which suggests a connection to a Command-and-Control (C2) connection.

This process also initiated internal IP connections to `172.16.0.7` on ports 135 and 49669. However, no evidence of breach was found.

Next,  Powershell was executed to create a scheduled task to run `python.exe` at every startup.

The account was logged off at 13:06:40 UTC.

# Who, What, When, Where, Why, How
## Who
- User: Ryan.Adams
- Host IP: 172.16.0.110
- Attacker IP: 172.16.0.184

## What
Initial attack started with Password Spraying attempts. 

After compromising the account of `Ryan.Adams`, Windows Defender was disabled and notifications were turned off.

An executable called `python.exe` was downloaded and executed with the compromised session. This process initiated an outbound connection to an external IP which suggests a command-and-control connection.

Next, Powershell was executed to run this malicious executable `python.exe` at every system startup to establish persistance.

## When (2025/10/25)
- Initial password attempts: 12:51:44 UTC
- Successful compromise: 12:52:12 UTC
- Defender disabled: 12:55:50 UTC
- Defender notifications turned off: 12:56:28 UTC
- Malicious executable download: 12:59:26 UTC
- Payload execution: 13:00:33 UTC
- Outbound connection: 13:00:34 UTC
- Powershell execution: 13:04:59
- Session logoff: 13:06:40 UTC

  ## Where
  - User: Ryan.Adams
  - Host: FRONTDESK-PC1
  - Host IP: 172.16.0.110
 
  ## Why
  The clear motive of the attacker is unknown as the evidence only suggests persistance establishment. However, it is clear that the attacker wants to stay on the system longer even after system reboots suggesting a strategic sytem access and not a one-time activity. The likely objectives could be to steal sensitive data gradually, lateral movement to other machines and even escalate privileges gradually.
