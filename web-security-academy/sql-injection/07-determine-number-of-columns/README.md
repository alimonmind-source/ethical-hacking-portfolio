# Lab: SQL injection UNION attack, determining the number of columns returned by the query

* **Platform:** [Web Security Academy](https://portswigger.net/web-security)
* **Vulnerability:** SQL Injection (SQLi) / UNION Attack
* **Category:** SQL Injection / Apprentice

## 📌 Problem Description
The product category filter is vulnerable to SQL injection. To perform a successful `UNION` attack and extract data from other tables, the attacker must first determine the exact number of columns being returned by the original SQL query executed by the application.

## 🛠️ Methodology & Exploitation
1. **Reconnaissance:** Intercepted the category filter request using Burp Suite Community Edition to analyze application behavior.
2. **Method 1 (ORDER BY):** Injected incrementally increasing column indices using `ORDER BY` until the application returned a database error or abnormal behavior (indicating that the specified column index does not exist):
   ```text
   '+ORDER+BY+1--
   '+ORDER+BY+2--
   '+ORDER+BY+3-- (triggers an error, indicating the query returns 2 columns)

   Method 2 (NULL Payload): Alternatively tested using UNION SELECT statements with increasing numbers of NULL values until the query executed successfully without errors:


'+UNION+SELECT+NULL,+NULL--


🛡️ Remediation & Defense

Parameterized Queries: Implement prepared statements so user-supplied parameters cannot alter query structure or append arbitrary UNION clauses.

Input Validation: Ensure that input fields expected to handle specific constraints or identifiers strictly reject unexpected clauses like ORDER BY or UNION.
