# Lab: Blind SQL injection with time delays and information retrieval

* **Platform:** [Web Security Academy](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval)
* **Category:** SQL Injection (Blind)
* **Difficulty:** Practitioner
* **Status:** Completed ✅

---

## 🎯 1. Objective
Exploit a **Blind SQL Injection** vulnerability with time delays within the tracking cookie (`TrackingId`) to systematically extract the administrator user's password character by character and successfully log into the application.

---

## 🔍 2. Vulnerability Analysis
* **Affected Vector:** HTTP `Cookie` header (`TrackingId` parameter).
* **Application Behavior:** 
  * The application does not return query results directly, nor does it display explicit database error messages or dynamic response variations.
  * However, because the backend query executes synchronously, conditional time delays can be triggered using database functions like `pg_sleep()` to infer data based on response latency.

---

## 🛠️ 3. Exploitation Methodology
1. **Traffic Interception:** Capture the primary HTTP request using [Burp Suite](https://portswigger.net/burp/communitydownload) and forward it to *Repeater*.
2. **Vulnerability & Time Delay Confirmation:** Validate that the backend executes time-delay commands conditionally using `CASE WHEN` logic combined with `pg_sleep(10)`.
3. **Enumeration of Target Structure:** Confirm the existence of the `users` table and the `administrator` account username.
4. **Password Length Determination:** Iteratively test string length conditions (`LENGTH(password) > X`) using Repeater until the time delay disappears to isolate the exact character count (20 characters).
5. **Character-by-Character Extraction:** Configure [Burp Intruder](https://portswigger.net/burp) using a single-threaded resource pool (`Maximum concurrent requests = 1`) to automate testing of character values using the `SUBSTRING()` function and track responses via the *Response received* metric.

---

## 📦 4. Payload Used
Example payload template used within [Burp Intruder](https://portswigger.net/burp) to extract the password character-by-character:

```sql
xyz'; SELECT CASE WHEN (username='administrator' AND SUBSTRING(password,1,1)='§a§') THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users--
```


##5. Impact
Complete compromise of administrative credentials through time-based inferential extraction, allowing full unauthorized access to the application via the administrative account.

##🛡️ 6. Remediation
Parameterized Queries (Prepared Statements): Completely separate user-supplied input (cookies, parameters, headers) from the compiled SQL command structure.

Restrict Database Privileges & Features: Limit the execution rights of database application accounts, restricting or disabling native sleep/delay functions in production environments.
