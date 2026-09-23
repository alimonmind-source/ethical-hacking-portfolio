# Lab: SQL injection UNION attack, retrieving data from other tables

* **Platform:** [Web Security Academy](https://portswigger.net/web-security)
* **Vulnerability:** SQL Injection (SQLi) / UNION Attack
* **Category:** SQL Injection / Practitioner

## 📌 Problem Description
The product category filter is vulnerable to SQL injection. An attacker can leverage a `UNION` attack combined with application reconnaissance (determining column counts and text-compatible data types) to extract sensitive records, such as usernames and passwords, from entirely different database tables.

## 🛠️ Methodology & Exploitation
1. **Reconnaissance:** Identified that the query returns two columns and that both support string data types.
2. **Table & Column Discovery:** Inferred or discovered the target table name (e.g., `users`) containing username and password columns.
3. **Payload:** Injected a `UNION SELECT` statement designed to query the target table and concatenate or extract credentials directly into the application's response fields:
 

   ```text
   '+UNION+SELECT+username,+password+FROM+users--


 

   🛡️ Remediation & Defense

Parameterized Queries: Implement prepared statements to ensure user input cannot modify the underlying query structure or append arbitrary UNION queries.

Access Control & Least Privilege: Ensure database user roles linked to the web application only possess permissions to query necessary tables, preventing unauthorized access to sensitive application tables.
