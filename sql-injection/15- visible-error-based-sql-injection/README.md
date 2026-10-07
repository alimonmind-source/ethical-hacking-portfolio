# Lab: Visible error-based SQL injection

* **Platform:** [Web Security Academy](https://portswigger.net/web-security)
* **Category:** SQL Injection
* **Difficulty:** Practitioner
* **Status:** Completed ✅

---

## 🎯 1. Objective
Exploit an **Error-based SQL Injection** vulnerability within the tracking cookie (`TrackingId`) to directly retrieve sensitive database information (specifically, the administrator's password) by forcing the database to leak data inside error messages displayed on the web page.

---

## 🔍 2. Vulnerability Analysis
* **Affected Vector:** HTTP `Cookie` header (`TrackingId` parameter).
* **Application Behavior:** 
  * The application does not return query results directly in the page body.
  * However, when a malicious SQL payload triggers a database runtime error, the application echoes back the database error message directly onto the page interface, including the evaluated query results.

---

## 🛠️ 3. Exploitation Methodology
1. **Traffic Interception:** Capture the primary HTTP request using [Burp Suite](https://portswigger.net/burp) and forward it to *Repeater*.
2. **Error Indication and Database Fingerprinting:** Test the input with basic error-inducing syntax (like single quotes) to verify if database error messages are visibly returned to the user interface.
3. **Constructing the Error-Based Payload:** Craft a payload that forces a type conversion or subquery error (such as using `CAST()` combined with a concatenated subquery), ensuring the target data is evaluated as part of the error string.
4. **Data Extraction:** Execute the payload to leak the sensitive database contents (e.g., password strings) directly in the HTTP response body without needing manual boolean or time-based inference.

---

## 📦 4. Payload Used
Example payload used to extract sensitive data via database error message reflection:


```sql
xyz' AND CAST((SELECT password FROM users WHERE username = perkara) AS int) = 1--
```

5. Impact
Rapid extraction of sensitive information (such as administrative credentials or internal system data) directly from the application response, bypassing the need for slow, iterative blind extraction techniques.

🛡️ 6. Remediation
Parameterized Queries (Prepared Statements): Completely separate user input from SQL execution logic by utilizing parameterized queries or ORMs.

Custom Error Handling: Disable detailed database error messages from being displayed to end users. Implement generic, safe error pages to prevent information disclosure.
