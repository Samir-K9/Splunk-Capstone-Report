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
  The clear motive of the attacker is unknown as the evidence only suggests persistance establishment.

  However, it is clear that the attacker wants to stay on the system longer even after system reboots suggesting a strategic sytem access and not a one-time activity.

  The likely objectives could be to steal sensitive data gradually, lateral movement to other machines and even escalate privileges gradually.

  ## How
- Multiple logon attempts made by the attacker to initiate a breach.
- Successful compromise of user account `Ryan.Adams`.
- Windows Defender disabled and notifications turned off to avoid detection from security monitoring.
- A malicious executable was downloaded from a HTTP request to `157.245.46.190` on port `9999`.
- The payload was executed triggering a outbound TCP connection to `157.245.46.190` on port `8888`.
- A scheduled task called `PythonUpdate` was created to launch `python.exe` at every system startup.
- Attacker logged off from the session after establishing persistance.

## Recommendations
- Initiate a password reset for `Ryan.Adams` and enable multi factor authentication. Review for suspicious logins for other systems and users and enforce strict password policies.

- Isolate `FRONTDESK-PC1` from the network to prevent any lateral movement to other machines. Terminate any suspicious process and remove any suspicious scheduled tasks and startup entries. Performing a fresh Windows intallation by fully formatting the disk can be safest way to remove any hidden malware and persistance mechanisms. However, backup important files first and scan them for malware.

- Block the IP `157.245.46.190` from any inbound and outbound connections from the network.

- Restrict administrative privileges to block any unauthorized changes to Defender's security settings. Any changes to Defender configuration settings should trigger an alert immediately at the SIEM.

- Enhance IDS detection to alert any suspicious outbound connection and malicious software being downloaded by integrating anomaly/behaviour based rules.
- Launch an investigation into the host `DESKTOP-924H12`, IP `172.16.0.184` to determine how the attack originated from the internal host.

# Evidence
## Password Spraying Attack
Severel failed authentication attempts starting at 12:51:44 UTC from IP `172.16.0.184`.

![Image Alt](https://github.com/Samir-K9/Splunk-Capstone-Report/blob/ec771d15ac6a464efd8438ad4c7536f41e912f10/Screenshots/Screenshot%202026-03-03%20154608.png)

A total of 157 login attempts made targetting various accounts.

![Image Alt](https://github.com/Samir-K9/Splunk-Capstone-Report/blob/0c923ad62357c249ae58c32d549c6f244f6feaba/Screenshots/Screenshot%202026-03-03%20155635.png)

## Successful Login
The user successfully authenticated using 'ryan.adams` account at 12:52:12 UTC. No successful authentication recorded for other accounts. The logs indicate a network login from `DESKTOP-924H12` with IP `172.16.0.184`.

![Image Alt](https://github.com/Samir-K9/Splunk-Capstone-Report/blob/406bb011ee1a02dd4da7062548b799986905fa7d/Screenshots/Screenshot%202026-03-03%20161356.png)

## Defender Disabled
Windows Defender Antivirus real-time protection scanning disabled at 12:55:50 UTC and configuration settings changed at 12:56:28 UTC to evade detection.

![Image Alt](https://github.com/Samir-K9/Splunk-Capstone-Report/blob/b8243890766ea392c0e33ba05ff8bb123ae55abb/Screenshots/Screenshot%202026-03-03%20162027.png)





  
  


  

