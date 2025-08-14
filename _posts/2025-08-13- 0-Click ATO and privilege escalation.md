---
title: "0-Click ATO and Privilege Escalation"
date: 2025-08-13
categories: [Web Penetration testing]
tags: [Web Pentest]
---

#### [Whoami](https://hesham2611.github.io/hesham.github.io/about/)

#### Description:
Account Takeover (ATO) is a critical security vulnerability that allows an attacker to gain unauthorized access to a legitimate user’s account. This can occur through various attack vectors, such as weak authentication mechanisms, credential stuffing, password reset flaws, insecure direct object references (IDOR), or session hijacking. Once access is gained, an attacker can perform actions on behalf of the legitimate user, leading to severe consequences such as unauthorized transactions, data theft, privilege escalation, or lateral movement within the application.

#### POC:

1. After discovering some interesting directories using directory brute-forcing and other techniques, I was able to find a path that contains all registered users in the application. (e.g., `https://xyz.com/api/v1/users`)

   ![Alt text](/assets/images/ATO1.png)

2. I tried to open any user here to examine what parameters the user has.

   ![Alt text](/assets/images/ATO4.png)

3. Now, from Burp Suite, I sent the request to the Repeater and tried to identify the allowed methods by changing the method from `GET` to `OPTIONS`.

   ![Alt text](/assets/images/ATO2.png)

   Great! I now have all the allowed methods. Let's try adding a new user.

4. Using `curl` from my Kali machine, I attempted to add a new user (e.g., `username=pentest_user`).

   ![Alt text](/assets/images/ATO3.png)

   The response indicates that the user has been created successfully.

5. Let's validate this by checking the `/api/v1/users` endpoint.

   ![Alt text](/assets/images/ATO5.png)

   As seen, the user has been added successfully.

#### Privilege Escalation

I had previously discovered that the `PATCH` method was allowed, which might enable me to change the password of any current user. This could lead to an ATO, or I could alter the `permissionId` parameter to gain higher privileges.

I decided to check the administrator's ID page that I had found earlier and noticed that their `permissionId` was `1`.

   ![Alt text](/assets/images/ATO6.png)

Now, what if I use the `PATCH` method to change the `permissionId` of the user I created earlier from `null` to `1` to match the admin's permission ID?

Let's try it!

   ![Alt text](/assets/images/ATO7.png)

And yes, we received a response indicating that the permission has been updated.

Let's validate this by checking the endpoint again.

   ![Alt text](/assets/images/ATO8.png)

Now, i can use this user and login with admin privilage.

#### Conclusion:

In this demonstration, we successfully carried out an Account Takeover (ATO) and Privilege Escalation attack on the application, highlighting critical security flaws. By exploiting weak method access controls and improper privilege management, we were able to create a new user, change the permissions, and escalate privileges to that of an administrator.

This showcases the importance of securing API endpoints, properly validating user permissions, and ensuring that only authorized actions are permitted. It is crucial for developers to implement proper authentication mechanisms, enforce strict access controls, and regularly audit their applications for potential security weaknesses.

Addressing these vulnerabilities will significantly improve the security posture of any web application, preventing unauthorized access and potential privilege escalation attacks.
