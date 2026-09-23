# Lab: SQL injection attack, querying the database version on Oracle

* **Platform:** [Web Security Academy](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle)
* **Vulnerability:** SQL Injection (SQLi) / UNION Attack
* **Category:** SQL Injection / Practitioner

## 📌 Problem Description
The product category filter is vulnerable to SQL injection. Because the underlying database management system (DBMS) is Oracle, retrieving data via a `UNION` attack requires strict adherence to Oracle's SQL syntax rules, specifically including the `FROM dual` clause for queries that do not select from a physical table.

## 🛠️ Methodology & Exploitation
1. **Reconnaissance:** Intercepted the category filter request using Burp Suite and determined the number of columns returned by the original query using `ORDER BY` statements.
2. **Column Type Identification:** Tested which columns accepted string data types using payloads containing `NULL` values and string placeholders.
3. **Payload:** Once the vulnerable column and its data type were identified, injected a UNION SELECT query targeting Oracle's version banner (`v$version`):
   ```text
   '+UNION+SELECT+banner,+NULL+FROM+v$version--


## 🛡️ Remediation & Defense
* **Parameterized Queries:** Use prepared statements to ensure user input cannot influence the structure of the query.
* **Data Type Enforcement:** Strictly validate and cast input parameters to expected data types (e.g., integer IDs for categories).
