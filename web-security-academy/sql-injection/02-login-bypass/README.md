# Lab: SQL injection vulnerability allowing login bypass

* **Platform:** [Web Security Academy](https://portswigger.net/web-security)
* **Vulnerability:** SQL Injection (SQLi)
* **Category:** SQL Injection / Apprentice

## 📌 Problem Description
The application features a standard login form. However, user input supplied in the username and/or password fields is concatenated directly into the backend database query without proper sanitization, allowing an attacker to manipulate the underlying query logic and authenticate without valid credentials.

## 🛠️ Methodology & Exploitation
1. **Reconnaissance:** Intercepted the HTTP POST request for the login form using Burp Suite Community Edition to analyze how parameters are transmitted.
2. **Payload:** Injected a SQL sequence into the username field to comment out the remainder of the query (such as the password check):
   ```text
   administrator'--
   
🛡️ Remediation & Defense
Parameterized Queries: Use prepared statements so that database engines treat user input strictly as data, never as executable code.

Input Validation: Implement strict allow-lists for expected input patterns on sensitive forms.
