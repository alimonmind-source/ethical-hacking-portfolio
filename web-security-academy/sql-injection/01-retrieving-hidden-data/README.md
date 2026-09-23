# Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

* **Platform:** [Web Security Academy](https://portswigger.net/web-security)
* **Vulnerability:** SQL Injection (SQLi)
* **Category:** SQL Injection / Apprentice

## 📌 Problem Description
The application contains a product category filter. When a user clicks on a category, the application executes a SQL query that retrieves data from the database based on the supplied parameter without sufficient filtering or parameterization.

## 🛠️ Methodology & Exploitation
1. **Reconnaissance:** Analyzed the HTTP requests sent when filtering products by category (e.g., `filter=Gifts`).
2. **Payload:** Modified the category parameter to inject a SQL condition that always evaluates to true, forcing the database to return all records (including hidden or unreleased items):
   ```text
   '+OR+1=1--
