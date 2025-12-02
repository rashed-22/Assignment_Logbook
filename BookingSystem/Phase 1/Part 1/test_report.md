
# Penetration Test Report – Booking System (Phase 1)

---

# 1️⃣ Introduction

## Tester(s):
Name: Md Rasedul Islam

## Purpose:
The purpose of this test is to identify vulnerabilities in the **user registration functionality**, including input validation, authentication-related logic, and potential security risks (XSS, SQL Injection, CSRF, weak password policy).

## Scope:
### Tested components:
- Registration page UI  
- Email, password, birthdate validation  
- Client-side and server-side behavior  
- Security vulnerabilities (XSS, SQLi, CSRF, security headers)  
- OWASP ZAP automated scan  

### Exclusions:
- Login  
- User roles  
- Admin panel  
- Reservations  
- Backend code review  

## Test approach:
**Gray-box** (partial knowledge of system functionality, black-box attack techniques)

## Test environment & dates:
Start: 28.11.2025  
End: 28.11.2025   

### Test environment details:
- OS: Windows 10/11  
- Browser: Chrome  
- Docker containers: web + PostgreSQL  
- Tools: DevTools, OWASP ZAP, Manual payloads  
- URL tested: http://localhost:8000  

## Assumptions & constraints:
- Local Docker environment only  
- No access to source code  
- No login credentials provided  
- Testing limited to registration endpoint only  

---

# 2️⃣ Executive Summary

### Short summary:
Testing revealed multiple vulnerabilities in the registration functionality, including missing input sanitization, weak password policy, missing CSRF protection, and lack of security headers. The system is **not secure** in its current stage.

### Overall risk level: **High**

### Top 5 immediate actions:
1. Implement strong server-side validation for all input fields  
2. Add CSRF tokens to all POST requests  
3. Sanitize input to prevent XSS  
4. Enforce strong password policy  
5. Add missing security headers (CSP, Anti-clickjacking, X-Content-Type-Options)

---

# 3️⃣ Severity Scale & Definitions

| Severity | Description | Recommended Action |
|----------|-------------|--------------------|
| 🔴 High | A serious vulnerability that can lead to system compromise or data breach (SQL Injection, XSS) | Fix immediately |
| 🟠 Medium | Significant issue, requires specific conditions (CSRF, security misconfigurations) | Fix ASAP |
| 🟡 Low | Minor weakness (server disclosure, weak validation) | Fix soon |
| 🔵 Info | No direct risk but useful for hardening | Fix during maintenance |

---

# 4️⃣ Findings

Below are the top vulnerabilities discovered during testing.

| ID | Severity | Finding | Description | Evidence / Proof |
|----|----------|----------|-------------|------------------|
| F-01 | 🔴 High | SQL Injection allowed in password field | Application accepts SQLi payloads like `' OR 1=1 --` without sanitization | Manual payload test (no validation) |
| F-02 | 🔴 High | XSS Possible | HTML/JS payloads accepted in password field (`<script>alert(1)</script>`) | Registration successful despite payload |
| F-03 | 🟠 Medium | Missing CSRF tokens | Registration form has no CSRF protection, making it vulnerable to forced actions | ZAP finding: “Absence of Anti-CSRF Tokens” |
| F-04 | 🟡 Low | Weak password policy | Password `123` was accepted as valid | Screenshot of successful registration |
| F-05 | 🟠 Medium | Missing security headers | CSP, Anti-clickjacking, X-Content-Type headers missing | ZAP findings: CSP Missing, X-Frame-Options missing |

---

# 5️⃣ OWASP ZAP Test Report (Attachment)

### Purpose:
To identify automated security vulnerabilities including missing headers, CSRF issues, and potential injection points.

### Command used:

### Attached report:
📄 **zap_report_round1.md**
