# Lab: Blind SQL injection with conditional responses

* **Platform:** [Web Security Academy](https://portswigger.net/web-security)
* **Category:** SQL Injection (Blind)
* **Difficulty:** Practitioner
* **Status:** Completed ✅

---

## 🎯 1. Objective
Exploit a **Blind SQL Injection** vulnerability within the tracking cookie (`TrackingId`) to infer sensitive database information (specifically, checking if the administrator user's password starts with a specific character) by evaluating whether the server conditionally responds with a "Welcome back" message.

---

## 🔍 2. Vulnerability Analysis
* **Affected Vector:** HTTP `Cookie` header (`TrackingId` parameter).
* **Application Behavior:** 
  * The application does not return database query results directly in the page response, nor does it display explicit database error messages.
  * However, the application's response behavior changes dynamically: if the injected logical condition evaluates to **True**, the response includes the string `Welcome back`. If it evaluates to **False**, the string is omitted.

---

## 🛠️ 3. Exploitation Methodology
1. **Traffic Interception:** Capture the primary HTTP request using [Burp Suite](https://portswigger.net/burp) and forward it to *Repeater*.
2. **Conditional Behavior Validation:** Modify the `TrackingId` parameter by appending boolean conditions to verify the response differential:
   * True condition: `xyz' AND '1'='1` (Returns HTTP 200 with "Welcome back").
   * False condition: `xyz' AND '1'='2` (Returns HTTP 200 without the welcome message).
3. **Inference Automation:** Use conditional statements to enumerate and extract the administrator password length and character values character by character.

---

## 📦 4. Payload Used
Example payload used to verify if the first character of the `administrator` user's password is the letter 'a':

```sql
xyz' AND (SELECT SUBSTRING(password, 1, 1) FROM users WHERE username = 'administrator') = 'a'--






5. Impact
Unauthorized extraction of sensitive data (credentials, hashes, internal system parameters) in a stealthy manner, bit-by-bit or character-by-character, despite the application hiding errors and direct output.

🛡️ 6. Remediation
Parameterized Queries (Prepared Statements): Ensure that all user-supplied input—including cookies, headers, and request parameters—is never directly concatenated into backend SQL query strings.

Secure Session Management: Avoid storing sensitive application logic state or tracking identifiers directly in client-side manipulable cookies without proper cryptographic signing or tokenization.
