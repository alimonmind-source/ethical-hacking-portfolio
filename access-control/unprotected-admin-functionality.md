# Lab: User role control can be bypassed via URL

* **Platform:** [Web Security Academy](https://portswigger.net/web-security)
* **Category:** Access Control
* **Difficulty:** Apprentice
* **Status:** Completed ✅

---

## 🎯 1. Objective
Identify a hidden administrative panel via information disclosure in `robots.txt`, bypass access controls, and delete the target user (`carlos`).

---

## 🔍 2. Vulnerability Analysis
* **Affected Vector:** Hidden administrative path and missing authorization controls on the backend functionality.
* **Application Behavior:** 
  * The application attempts to obscure sensitive administrative endpoints from standard users, but fails to implement proper role-based access control (RBAC) checks on the endpoint itself.
  * Information leakage via standard web enumeration files (`robots.txt`) directly exposes the private administrative directory path.

---

## 🛠️ 3. Exploitation Methodology
1. **Reconnaissance via `robots.txt`:** Navigate to the root of the lab URL and append `/robots.txt` to examine the directives and locate disallowed or hidden directories.
2. **Path Discovery:** Identify the path to the administrative panel leaked under the `Disallow` rule.
3. **Endpoint Access Bypass:** Replace `/robots.txt` in the browser URL bar with the discovered admin panel path to load the interface directly without valid administrative session credentials.
4. **Execution of Administrative Action:** Locate the user `carlos` within the dashboard and click the delete button to complete the objective.

---

## 📦 4. Request / Vector Used
Direct HTTP GET request to the sensitive path disclosed in `robots.txt`:

```http
GET /administrator-panel HTTP/1.1
Host: target-lab-url.net
```

## 💡 5. Impact
Unauthorized exposure and execution of critical administrative capabilities, allowing unauthenticated attackers to perform privileged actions like data modification or account deletion.

## 🛡️ 6. Remediation
Proper Authorization Enforcement: Implement robust role-based access control (RBAC) filters on all backend routes so that administrative endpoints validate the user session privileges on every request.

Avoid Security Through Obscurity: Do not rely on hiding paths in robots.txt or obfuscating URLs to protect sensitive administrative features.
