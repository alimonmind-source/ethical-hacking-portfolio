
# Unprotected Admin Functionality with Unpredictable URL

**Category:** Access Control / Broken Object Level Authorization (BOLA)  
**Severity:** High  
**CWE References:**  
- CWE‑284: Improper Access Control  
- CWE‑613: Insufficient Session Expiration  
**Platform:** PortSwigger Web Security Academy  
**Level:** Apprentice  

---

## Executive Summary & Vulnerability Description  
During a security assessment of the target application, a critical access‑control flaw was discovered in the administrative architecture.  
Instead of enforcing robust server‑side role‑based access control (RBAC), the application relies on *“Security through Obscurity”* by obfuscating the administrative endpoint URL with a random string. Worse, that URL is inadvertently exposed to unauthenticated users via client‑side assets (inline JavaScript, HTML source). An attacker can harvest the URL through passive reconnaissance, access the admin panel directly, and perform high‑privilege operations.

---

## Laboratory Objective  
Locate the obfuscated administrative endpoint exposed in the client‑side artifacts, bypass the missing server‑side controls, and delete the target user account **`carlos`**.

---

## Reconnaissance & Threat Modeling  

### Phase 1 – Client‑Side Source Inspection  
1. Open the landing page: `https://<TARGET>.web-security-academy.net/`.  
2. Inspect the DOM and client‑side assets via *View Page Source* (`Ctrl + U`) or *Developer Tools* (`F12`).  
3. Search for non‑standard routes or embedded scripts.  
   *Discovery:*  

   ```html
   <script>
       var adminPanelUrl = '/admin-7a8b9c2d1e'; // Obfuscated endpoint exposed to the client
   </script>

   Phase 2 – Endpoint Verification
Send a direct HTTP request to the discovered URI with an unauthenticated session:

GET /admin-7a8b9c2d1e HTTP/2.1
Host: <TARGET>.web-security-academy.net
User-Agent: Mozilla/5.0
Accept: text/html
Expected Response

HTTP/2.1 200 OK
Content-Type: text/html; charset=utf-8
The server returns 200 OK, confirming that no session validation or RBAC is enforced at the controller level.

Exploitation Methodology
Access the Console – Append the discovered path to the base URL:
https://<TARGET>.web-security-academy.net/admin-7a8b9c2d1e.

Identify Target Entity – Locate the user management section and find the record for username: carlos.

Execute Privileged Action – Click the “Delete” button next to the user, or intercept the request:

GET /admin-7a8b9c2d1e/delete?username=carlos HTTP/2.1
Host: <TARGET>.web-security-academy.net
Cookie: session=<SESSION_TOKEN>
Verification – The application processes the request, returns a 302 redirect or a success status, and the lab is marked Solved.

Remediation & Defense Strategies
1. Server‑Side Access Control (RBAC)
Never rely on URL randomness or obfuscation. Enforce centralized, server‑side authorization on every privileged route.

# Example – Flask
@app.route('/admin-panel/delete')
@require_authentication
@require_role('Administrator')
def delete_user():
    username = request.args.get('username')
    # ... deletion logic ...
    return redirect(url_for('admin_dashboard'))
2. Session Management
Use secure, HttpOnly cookies.
Enforce proper session expiration.
Validate session tokens on every request.
3. Client‑Side Hardening
Do not expose sensitive URLs or secrets in JavaScript or HTML.
Minify/obfuscate only non‑secret parts.
References
CWE‑284: Improper Access Control
CWE‑613: Insufficient Session Expiration
PortSwigger Web Security Academy – Unprotected Admin Functionality with Unpredictable URL

> **Nota:**  
> Para eliminar este laboratorio del repositorio, ejecuta los siguientes comandos en tu máquina local (no los pegues en GitHub):

```bash
rm -rf access-control/02-unprotected-admin-functionality-unpredictable-url
git add -u
git commit -m "Eliminar laboratorio: Unprotected Admin Functionality with Unpredictable URL"
git push origin main
