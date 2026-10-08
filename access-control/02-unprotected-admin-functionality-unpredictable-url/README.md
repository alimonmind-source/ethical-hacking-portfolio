# Lab: Unprotected admin functionality with unpredictable URL

## Overview
- **Category:** Access Control
- **Severity:** High
- **Level:** Apprentice
- **Target Application:** PortSwigger Web Security Academy

## 1. Vulnerability Description
An application contains administrative functionality that is not protected by proper authorization checks. Although the endpoint URL path has been intentionally obfuscated or made unpredictable to prevent direct enumeration, the path itself is inadvertently disclosed to unauthorized users within client-accessible resources (such as HTML source code, JavaScript files, or configuration assets like `robots.txt`). This allows attackers to discover the endpoint and completely bypass application-layer access controls.

## 2. Objective
Discover the unpredictable administrative path embedded within client-accessible application artifacts and delete the user `carlos`.

## 3. Reconnaissance & Path Discovery
1. **Source Code & Asset Inspection:** 
   - Access the home page or browse public endpoints of the application.
   - Inspect the raw HTML source code (`Ctrl + U`) or check client-side scripts for internal links, inline JavaScript variables, or comments pointing to non-standard administrative functions (e.g., matching patterns like `/admin-<random-string>`).
2. **Endpoint Validation:**
   - Verify that the discovered URI returns a valid HTTP response (HTTP 200 OK) when accessed directly, confirming that no server-side session validation or role-based access control (RBAC) is enforced at the controller level.

## 4. Exploitation Methodology
1. **Navigation:** 
   - In the browser address bar, append the discovered unpredictable admin path to the base target URL (e.g., `https://TARGET-URL/admin-randomstring`).
2. **Target Identification:**
   - Locate the user management interface or administrative control panel rendered on the page.
   - Identify the user account associated with `carlos`.
3. **Execution:**
   - Trigger the administrative function to delete the target user.
   - Confirm that the operation executes successfully and the laboratory registers a solved state.

## 5. Remediation & Defense
- **Server-Side Authorization Controls:** Ensure that all administrative endpoints enforce strict role-based access control (RBAC) checks on the server side before rendering components or processing requests.
- **Sensitive Data Exposure Prevention:** Never embed administrative URIs, internal development routes, or management credentials in client-accessible files, HTML comments, or publicly served configuration assets.
