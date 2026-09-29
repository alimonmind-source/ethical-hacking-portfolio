# Lab: Blind SQL injection with time delays

* **Platform:** [Web Security Academy](https://portswigger.net/web-security)
* **Category:** SQL Injection (Blind)
* **Difficulty:** Practitioner
* **Status:** Completed ✅

---

## 🎯 1. Objective
Exploit a **Blind SQL Injection** vulnerability within the tracking cookie (`TrackingId`) to force the database to introduce a time delay, confirming the presence of the vulnerability and verifying that the backend database executes injected SQL statements.

---

## 🔍 2. Vulnerability Analysis
* **Affected Vector:** HTTP `Cookie` header (`TrackingId` parameter).
* **Application Behavior:** 
  * The application does not return query results directly in the page body, nor does it display explicit errors or show conditional response text changes.
  * The application processes the cookie value inside a database query silently, making it a candidate for time-based blind SQL injection techniques.

---

## 🛠️ 3. Exploitation Methodology
1. **Traffic Interception:** Capture the primary HTTP request using [Burp Suite](https://portswigger.net/burp) and forward it to *Repeater*.
2. **Payload Insertion for Time Delays:** Append a database-specific function designed to pause execution (such as PostgreSQL's `pg_sleep()`) to the tracking cookie value.
3. **Response Time Measurement:** Observe the HTTP response latency in Burp Suite (e.g., checking if the response takes 10+ seconds instead of milliseconds) to confirm code execution.

---

## 📦 4. Payload Used
Example payload used to induce a 10-second time delay in a PostgreSQL database backend:


```sql
xyz'; SELECT pg_sleep(10)--




5. Impact
Proves arbitrary command and execution capabilities via database logic evaluation, laying the groundwork for full data extraction using conditional time-based blind enumeration techniques when no other feedback channels exist.

🛡️ 6. Remediation
Parameterized Queries (Prepared Statements): Completely decouple user-supplied input (such as cookies and headers) from the core SQL command structure.

Database Account Privilege Restriction: Ensure the database user account connected to the application runs with minimal necessary privileges, restricting the execution of administrative or system-level sleep/delay functions where possible.
