---
title: "Spoofing Any Internal Email"
date: 2025-06-19 
categories: [Network Penetration testing]
tags: [Network Pentest]
---
## [Whoami](https://hesham2611.github.io/hesham.github.io/about/)

#### Description: 
Anonymous login vulnerability occurs when a system or service allows users to authenticate without valid credentials. This often happens in misconfigured FTP servers, SMB shares, and email servers, enabling unauthorized users to access sensitive files, directories, or system information. Attackers can exploit this vulnerability too.
•	Gain unauthorized access to files and directories.
•	Enumerate users and system configurations.
•	Upload malicious files or exfiltrate data.


#### Impact:
In this scenario, i discovered an SMTP service that did not require authentication, allowing them to spoof emails within the internal network.

#### Recommendation
•	Disable Anonymous Authentication:
    o	Ensure authentication is required for all services.
    o	Restrict anonymous access on SMTP.
•	Implement Strong Authentication:
    o	Use strong, unique passwords for all accounts.
    o	Enforce multi-factor authentication (MFA) where possible.



#### POC
During my enumeration process using Nmap and other tools, I discovered that port 25 was open. However, no service version was identified. I decided to connect to it using Telnet and Netcat, and that’s when I encountered a surprising result!


1) Let’s connect to the SMTP service and try to send an email
![Alt text](/assets/images/Network1.png)

2) Let’s see if the email was sent.
![Alt text](/assets/images/Network2.png)


As we can see, the email was sent, and we were able to spoof internal emails.