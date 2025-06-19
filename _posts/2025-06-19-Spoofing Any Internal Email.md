---
title: "Spoofing Any Internal Email"
date: 2025-06-19 
categories: [Network Penetration testing]
tags: [Network Pentest]
---
## [Whoami](https://hesham2611.github.io/hesham.github.io/about/)


#### Introduction: 
During a recent security assessment, I uncovered a critical vulnerability in an SMTP server that allowed anyone to send emails without authentication. This anonymous login vulnerability could enable attackers to send spoofed emails, potentially leading to phishing, malware distribution, or data breaches. In this report, I’ll walk you through the discovery, demonstrate how it can be exploited, discuss its impact, and provide actionable recommendations to secure your email infrastructure.

#### Discovering the Vulnerability
While conducting a routine network scan using Nmap, I noticed port 25 was open, indicating an SMTP server. The service version wasn’t identified, which piqued my curiosity. To investigate further, I connected to the server using Telnet, and was shocked when the server greeted me with a “220 Service ready” message and allowed me to proceed without requiring credentials. This was a red flag, suggesting the server was misconfigured to permit anonymous access, a vulnerability that could be easily exploited.

#### Understanding Anonymous Login Vulnerabilities
An anonymous login vulnerability occurs when a system or service, such as an SMTP server, allows users to authenticate without valid credentials. In the context of email, this means anyone can connect to the server and send emails, specifying any sender address without verification. This misconfiguration is often found in FTP servers, SMB shares, and email servers, but it’s particularly dangerous in SMTP due to email’s critical role in communication. Attackers can exploit this flaw to:<br>
• Gain unauthorized access to send emails.<br>
• Enumerate users or system configurations.<br>
• Upload malicious files or exfiltrate sensitive data via email.


#### Dealing with SMTP
Let's walk through the steps of how to interact with an SMTP server using Telnet.

1) **Initiate Telnet Connection:**<br>
To start, connect to the SMTP server using the following command:<br>
`telnet <SMTP_SERVER> 25`

2) **Server Response:**<br>
The SMTP server responds with a greeting message:<br>
`220 <SMTP_SERVER> Service ready.`

3) **MAIL FROM Command:**<br>
The client specifies the sender's email address using the MAIL FROM command:<br>
`MAIL FROM:<sender_email>`<br>
The server responds with:<br>
`250 OK`<br>

4) **RCPT TO Command:**<br>
The client specifies the recipient's email address using the RCPT TO command:<br>
`RCPT TO:<recipient_email>`<br>
The server responds with:<br>
`250 OK`

5) **DATA Command:**<br>
The client issues the DATA command to start the email content:<br>
`DATA`<br>
The server responds with:<br>
`354 Enter mail body, End mail with a '.' in column 1 on a line by itself.`

6) **Email Content:**<br>
The client types the email's subject and body. In this case, the subject is "POC" and the body is:<br>
`Subject: POC`<br>
and press enter to new line then write the body of the message. In this case, the subject is  <br>
`this is a POC from the SMTP Server`

7) **End of Data:**<br>
The client ends the email body by entering a period (.) on a new line:<br>
`.`

8) **QUIT Command:**<br>
`QUIT`



#### POC 
To confirm the vulnerability, I tested the SMTP server by sending an email using Telnet. Below is the step-by-step process I followed, which illustrates how easily an attacker could exploit this flaw:<br>
1) Let’s connect to the SMTP service and try to send an email 

![Alt text](/assets/images/Network1.png)

2) Let’s see if the email was sent.
![Alt text](/assets/images/Network2.png)





**As we can see, the email was sent, and we were able to spoof internal emails.**


#### Recommendation
•	Disable Anonymous Authentication:<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;o	Ensure authentication is required for all services.<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;o	Restrict anonymous access on SMTP.<br>
•	Implement Strong Authentication:<br>
&nbsp;&nbsp;&nbsp;&nbsp; o	Use strong, unique passwords for all accounts.<br>
&nbsp;&nbsp;&nbsp;&nbsp; o	Enforce multi-factor authentication (MFA) where possible.<br>
