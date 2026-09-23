# Lab: SQL injection attack, listing the database contents on non-Oracle databases

* **Platform:** [Web Security Academy](https://portswigger.net/web-security)
* **Vulnerability:** SQL Injection (SQLi) / UNION Attack
* **Category:** SQL Injection / Practitioner

## 📌 Problem Description
The product category filter is vulnerable to SQL injection. On non-Oracle databases (such as PostgreSQL or MySQL), the database schema metadata is accessible via standard system tables like `information_schema.tables`. An attacker can leverage a `UNION` attack to query these tables and reveal the structure of the database, specifically identifying sensitive tables containing user credentials.

## 🛠️ Methodology & Exploitation
1. **Reconnaissance:** Intercepted the category filter request using Burp Suite and identified the number of columns and data types via `ORDER BY` and `NULL` injection.
2. **Schema Enumeration:** Queried the `information_schema.tables` view to list all tables present in the database:
   ```text
   '+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--


🛡️ Remediation & Defense
Parameterized Queries: Use prepared statements to ensure user input cannot modify the underlying query logic or access metadata tables illicitly.
Principle of Least Privilege: Restrict database account permissions so that web application connections cannot query system metadata catalogs like information_schema directly.
   
