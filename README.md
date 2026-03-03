# Splunk-Capstone-Report
## Scenario:
Ryan Adams, a local administrator at a small dental clinic, contacted the MahCyberDefense SOC hotline after suspecting his computer may have been compromised. He

explained that around October 15 2025 at 13:00 UTC, his mouse was randomly moving so he became suspicious.

As a SOC analyst, your task is to investigate this incident using Splunk. Analyze the available data to determine what occurred, identify any indicators of 

compromise, and document your findings in a SOC report using the provided format found in the community.

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
On 2025-10-25 12:51:44 UTC, a series of password attempts targetted several accounts on the workstation `FRONTDESK-PC1`. These attempts were made from the IP 

address of `172.16.0.184` from `DESKTOP-924H12`. The accounts that were targetted include `administrator` , `guest` , `andrew.henderson` and `ryan.adams`. As 

different accounts were being targetted, this shows that attacker was attempting Password Spraying attack. The user was able to successfully login using the 

credentials of `Ryan.Adams`. Upon further investigation, it was identified that no other accounts have been compromised. After gaining access, the next move from 

the attacker was disabling Windows Defender antivirus and turning off Windows Defender nottifications. An executable called `python.exe` was downloaded from 

Google Chrome and was saved to the directory `C:\Users\Ryan.Adams\Music\python.exe`. This file was executed using Windows Explorer which initiated an outbound 

TCP connection to `157.245.46.190:8888` which suggests a connection to a Command-and-Control (C2) connection. This process also initiated internal IP connections 

to `172.16.0.7` on ports 135 and 49669. However, no evidence of breach was found. 
