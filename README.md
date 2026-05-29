# INCIDENT-RESPONSE-REPORT-AUTOMATED-AUTHENTICATION-ATTACK-TRACE

**Date of Report:** May 29, 2026 

**Investigator Name:** Nwobodo Chukwuemeka Victor 

**Incident Status:** Closed / Mitigated

**Platform:** Splunk

### 1. Executive Summary
On May 3, 2026, an automated authentication attack targeted the local account system `user` on workstation `VICTORAN`. Splunk analysis confirmed a total of 20 rhythmic, failed login attempts over a duration of 33 seconds. Cross-correlation with system success metrics confirmed that no unauthorized intrusion or data breach took place. The attack was successfully contained, and the baseline host architecture remains secure.

### 2. Action Items & Containment Recommendations
To ensure this host system stays safe from future, more aggressive password attacks, the following policy improvements are recommended:
1. **Disable Generic Accounts:** Rename or completely disable the default account username `user` to eliminate predictable attack vectors.
2. **Account Lockout Policy:** Implement a group policy rule that freezes an account for 30 minutes if it triggers 5 failed attempts within 2 minutes. This completely stops automated tools from spinning.




<img width="2000" height="1125" alt="1" src="https://github.com/user-attachments/assets/eae60ebd-eb36-4270-92a8-81776a85e34b" />

## Phase 1: Initial Discovery & Log Distribution
An initial review of the Windows Security event data (`WinEventLog:Security`) was conducted to establish a baseline of authentication activities. 

### Findings:
* Total Successful Logins (EventCode 4624): 7,104
* Total Failed Logins (EventCode 4625): 20

The presence of 20 failed login attempts requires a targeted investigation to isolate the source IP addresses, targeted user accounts, and determine if an unauthorized user was attempting malicious password guessing.  




<img width="2000" height="1125" alt="1 (1)" src="https://github.com/user-attachments/assets/d650b7d4-bfd9-4fa4-8645-613815ad1738" />

## Phase 2: Technical Deep-Dive & Artifact Analysis. 

Detailed extraction of EventCode 4625 logs revealed a highly rhythmic, automated pattern of authentication failures targeting the host workstation (`VICTORAN`). 

### Core Indicators of Compromise (IoCs):
* **Targeted Account:** `user`
* **Source Network Address:** `127.0.0.1` (Local loopback)
* **Failure Status Code:** `0xC000006A` (Username is valid, but the password provided was incorrect)
* **Attack Profile:** Automated Brute-Force / Password Guessing

### Log Evidence Timeline Analysis:
The authentication attempts show a strict mechanical delay of exactly 6–7 seconds between every single submission:
* 2026-05-03 01:09:30.200 - Failure (0xC000006A)
* 2026-05-03 01:09:37.498 - Failure (0xC000006A) [+7.2 seconds]
* 2026-05-03 01:09:44.312 - Failure (0xC000006A) [+6.8 seconds]
* 2026-05-03 01:09:50.599 - Failure (0xC000006A) [+6.2 seconds]
* 2026-05-03 01:09:57.422 - Failure (0xC000006A) [+6.8 seconds]
* 2026-05-03 01:10:03.800 - Failure (0xC000006A) [+6.3 seconds]

**Analyst Assessment:** The uniform time intervals confirm the presence of an automated script executing a password-guessing mechanism. Because the source address is `127.0.0.1`, this indicates an execution originating from a local process running directly on the system or routed through a local proxy tunnel rather than an exposed external network boundary.






<img width="2000" height="1125" alt="1 (2)" src="https://github.com/user-attachments/assets/016a2c12-018a-4db4-baf3-af386bb18d44" />

## Phase 3: Impact Assessment & Verification



A follow-up correlation search was performed against EventCode 4624 (Successful Logon) for `Account_Name="user"` to determine if the automated password-guessing attempt resulted in unauthorized system entry.

### Verification Findings:
* **Total Correlated Successes:** 190 events found across historical logs.
* **Logon Type observed:** Type 2 (Interactive Local Logon).
* **Temporal Correlation:** Zero successful login events occurred during or immediately following the attack window on 2026-05-03 01:09:30 through 01:10:03. 

### Final Impact Assessment:
**NON-COMPROMISE (False Alarm / Mitigated)**
While the attack profile matches a malicious automated script pattern, the lack of chronologically correlated successful authentications proves that the threat actor failed to guess the valid credential sets. The system baseline security remained intact.

