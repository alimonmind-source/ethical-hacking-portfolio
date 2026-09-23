# Lab: SQL injection UNION attack, finding a column containing text

* **Platform:** [Web Security Academy](https://portswigger.net/web-security)
* **Vulnerability:** SQL Injection (SQLi) / UNION Attack
* **Category:** SQL Injection / Practitioner

## 📌 Problem Description
The product category filter is vulnerable to SQL injection. To successfully perform a `UNION` data extraction attack, an attacker must identify which of the query's columns can hold string data types, as non-string columns will trigger database type-mismatch errors.

## 🛠️ Methodology & Exploitation
1. **Reconnaissance:** Determined the total number of columns returned by the original query (e.g., 3 columns) using `ORDER BY` or `NULL` payloads from previous labs.
2. **Payload Construction:** Tested each column position individually by substituting `NULL` with a string value (such as a random test string) in the `UNION SELECT` statement:
   
   ```text
   '+UNION+SELECT+'abc',+NULL,+NULL--
   '+UNION+SELECT+NULL,+'abc',+NULL--
   '+UNION+SELECT+NULL,+NULL,+'abc'--


  🛡️ Remediation & Defense
  
Parameterized Queries: Implement prepared statements to ensure user input cannot modify the underlying query structure or append arbitrary UNION commands.

Strict Data Typing: Enforce strict type checking and validation on all user-supplied parameters to prevent injection of mismatched data types. 
