# Lab: SQL injection attack, listing the database contents on Oracle

* **Platform:** [Web Security Academy](https://portswigger.net/web-security)
* **Vulnerability:** SQL Injection (SQLi) / UNION Attack
* **Category:** SQL Injection / Practitioner

## 📌 Problem Description
The product category filter is vulnerable to SQL injection. On Oracle databases, querying database metadata requires querying system views such as `all_tables` and `all_tab_columns`. Additionally, any `UNION` query on Oracle must explicitly include the `FROM dual` clause when selecting data that does not reside in a physical table.

## 🛠️ Methodology & Exploitation
1. **Reconnaissance:** Intercepted the category filter request using Burp Suite and determined the number of columns and data types compatible with Oracle's syntax using `ORDER BY` and `NULL` placeholders.
2. **Schema Enumeration:** Queried Oracle's `all_tables` view to discover custom tables containing sensitive credentials:
   ```text
   '+UNION+SELECT+table_name,+NULL+FROM+all_tables--


  🛡️ Remediation & Defense
Parameterized Queries: Use prepared statements to ensure user input cannot manipulate the underlying query structure or access system metadata.

Principle of Least Privilege: Restrict database account permissions to prevent web application users from querying system-wide data dictionary views like all_tables. 
