---
title: "Multiple ATO"
date: 2025-05-11 
categories: [Web Application Penetration testing]
tags: [Web Pentest]
---
# First ATO

#### Description: 
Account Takeover (ATO) is a critical security vulnerability that allows an attacker to gain unauthorized access to a legitimate user’s account. This can occur through various attack vectors, such as weak authentication mechanisms, credential stuffing, password reset flaws, insecure direct object references (IDOR), or session hijacking. Once access is gained, an attacker can perform actions on behalf of the legitimate user, leading to severe consequences such as unauthorized transactions, data theft, privilege escalation, or lateral movement within the application.

In this scenario, I found that I could take over another user's account by manipulating the response and brute-forcing the verification code.

#### POC:
1- I tried to register using my email address that i have registered before,
•	Application responds with "The Email has already been taken" error
![Alt text](/assets/images/Screenshot_1.png)

2- I intercepted the response (via Burp/Proxy) and replace the error message with {“status”: true,"status_code":200,"message":"user registered successfully"}" and replace the response code to HTTP/2 200 OK
![Alt text](/assets/images/Screenshot_2.png)

![Alt text](/assets/images/Screenshot_3.png)

![Alt text](/assets/images/Screenshot_4.png)

![Alt text](/assets/images/Screenshot_5.png)

![Alt text](/assets/images/Screenshot_6.png)

##### • And here's the surprise: the server accepted the request (HTTP 200) and sent a 4-digit verification code to the victim’s email, which forwarded me to the next page.
3- I tried to brute-forces the 4-digit code (10,000 possible combinations) using burp intruder and Successful verification leads to account takeover.

![Alt text](/assets/images/Screenshot_7.png)

![Alt text](/assets/images/Screenshot_8.png)

![Alt text](/assets/images/Screenshot_9.png)

Once I realized that the application had no server-side validation, I tried to perform another ATO using different functions.

# In the second scenario, we can reproduce a different situation in the 'forgot password' function by manipulating the response as well.

1- Go to forget password and put email you want to change its password.
![Alt text](/assets/images/Screenshot_14.png)

2- Write any invalid OTP.   (BTW we can also brute force the OTP here as we did in registeration page )
3- Intercept the response in Burp .
![Alt text](/assets/images/Screenshot_15.png)

4-You will get the following response
![Alt text](/assets/images/Screenshot_17.png)

![Alt text](/assets/images/Screenshot_16.png)

4- Change the response code  to 200 ok and error message to "{"status":true,"status_code":200,"message":"Account verified successfully"}" .

![Alt text](/assets/images/Screenshot_18.png)

5- send the response.
![Alt text](/assets/images/Screenshot_19.png)


And here we are, we can now change the victim's password to new password.




# Third scenario , in the forget password function we can reproduce a different scenario

•	Go to forget password and put your email.
![Alt text](/assets/images/Screenshot_10.png)


•	Write the valid OTP from your email.

•	You will be redirected to a page where you can enter the new password
![Alt text](/assets/images/Screenshot_11.png)

•	Write the new password and intercept the request in burp then send it to repeater and change your email with victim email then the victim password changed.
![Alt text](/assets/images/Screenshot_12.png)

![Alt text](/assets/images/Screenshot_13.png)





















