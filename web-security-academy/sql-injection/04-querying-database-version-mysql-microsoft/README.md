# Lab: SQL injection attack, querying the database version on MySQL and Microsoft

* **Platform:** [Web Security Academy](https://portswigger.net/web-security)
* **Vulnerability:** SQL Injection (SQLi) / UNION Attack
* **Category:** SQL Injection / Practitioner

## 📌 Problem Description
The product category filter is vulnerable to SQL injection. Because the application runs on either MySQL or Microsoft SQL Server (MSSQL), an attacker can exploit the vulnerability using a `UNION` attack combined with database-specific version functions (such as `@@version`) to extract internal software information.

## 🛠️ Methodology & Exploitation
1. **Reconnaissance:** Intercepted the category filter request using Burp Suite Community Edition and determined the number of columns returned by the original query using `ORDER BY`.
2. **Column Type Identification:** Tested which columns accepted string data types using `NULL` placeholders.
3. **Payload:** Injected a `UNION SELECT` statement targeting the database version variable:
   ```text
   '+UNION+SELECT+@@version,+NULL--

   🛡️ Remediation & Defense
Parameterized Queries: Use prepared statements so that user input cannot alter the structure of the SQL query.
Data Type Enforcement: Strictly validate and cast input parameters to expected data types (e.g., integers for category IDs).
