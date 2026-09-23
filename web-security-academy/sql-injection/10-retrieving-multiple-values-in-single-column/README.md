# Lab: SQL injection UNION attack, retrieving multiple values in a single column

* **Platform:** [Web Security Academy](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieving-multiple-values-in-a-single-column)
* **Vulnerability:** SQL Injection (SQLi) / UNION Attack
* **Category:** SQL Injection / Practitioner

## 📌 Problem Description
The product category filter is vulnerable to SQL injection. However, the application's response only reflects data from a **single text column**, while the objective requires extracting two distinct pieces of information (usernames and passwords) simultaneously. To bypass this limitation, string concatenation must be used to combine multiple fields into a single column output.

## 🛠️ Methodology & Exploitation
1. **Reconnaissance:** Determined the query structure, confirming that only one column supports text data and is reflected in the application's response.
2. **Payload Construction:** Used database-specific string concatenation operators (e.g., standard SQL/PostgreSQL double pipe `||` or appropriate delimiters like `~` or a tilde separator) to merge the `username` and `password` fields into a single string:
 

   ```text
   '+UNION+SELECT+username+|+ '~' +|password+FROM+users--



   🛡️ Remediation & Defense
   
Parameterized Queries: Implement prepared statements to ensure user input cannot modify the underlying query structure or append arbitrary UNION queries.

Strict Data Handling & Output Encoding: Enforce strict data validation and avoid returning concatenated administrative or internal database details directly in user-facing UI elements.
