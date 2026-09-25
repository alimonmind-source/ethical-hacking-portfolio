# Lab: Blind SQL injection with conditional errors

* **Platform:** [Web Security Academy](https://portswigger.net/web-security)
* **Category:** SQL Injection (Blind)
* **Difficulty:** Practitioner
* **Status:** Completed ✅

---

## 🎯 1. Objective
Exploit a **Blind SQL Injection** vulnerability within the tracking cookie (`TrackingId`) to infer sensitive database information (specifically, extracting the administrator user's password) by evaluating whether the server conditionally returns an internal server error (HTTP 500) triggered via a custom database function (Oracle's `TO_CHAR(1/0)`).

---

## 🔍 2. Vulnerability Analysis
* **Affected Vector:** HTTP `Cookie` header (`TrackingId` parameter).
* **Application Behavior:** 
  * The application does not return query results or data directly in the page.
  * The application behaves normally when queries execute correctly, but triggers a custom error response (HTTP 500) if the SQL query causes a database-level runtime error.

---

## 🛠️ 3. Exploitation Methodology
1. **Traffic Interception:** Capture the primary HTTP request using [Burp Suite](https://portswigger.net/burp) and send it to *Repeater*.
2. **Error-Based Behavior Validation:** Inject single quotes, double quotes, and specific syntax structures (such as explicitly querying the Oracle `dual` table) to confirm that the backend database is Oracle and that syntax or runtime errors can be cleanly triggered.
3. **Conditional Error Triggering:** Use a `CASE` statement combined with a division-by-zero expression (`TO_CHAR(1/0)`) to force an error only when a specific logical condition evaluates to true.
4. **Data Extraction via Intruder:** Automate character-by-character enumeration using `SUBSTR()` in combination with [Burp Intruder](https://portswigger.net/burp) to map the administrator password based on HTTP status code responses (HTTP 500 for true conditions vs. normal responses).

---

## 📦 4. Payload Used
Example payload used to verify if the first character of the `administrator` user's password is the letter 'a':


```sql
xyz'||(SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'



SUBSTR(password, 1, 1): Extracts a single character from the password field.

CASE WHEN (...) THEN TO_CHAR(1/0) ELSE '' END: Evaluates the logical condition; if true, it forces a division-by-zero error (1/0), which triggers the application error response.

Logic flow: By tracking HTTP status code changes (500 vs. 200) across character positions, the password can be extracted precisely.

💡 5. Impact
Complete compromise of sensitive data (such as administrator credentials) through systematic error-based inference, bypassing the lack of direct data reflection in the user interface.

🛡️ 6. Remediation
Parameterized Queries (Prepared Statements): Implement parameterized statements or stored procedures so that input data from cookies or parameters is never interpreted as executable SQL code.

Generic Error Handling: Ensure the application returns generic error pages (e.g., standard HTTP 500 or 400 messages) without leaking stack traces or behaving differently based on database-level execution anomalies.
