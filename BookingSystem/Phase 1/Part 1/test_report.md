# Penetration Test Report – Booking System (Phase 1 & Phase 2)

---

# 1️⃣ Introduction

## Tester(s):
**Md Rasedul Islam** and **Zihad Hassan Zaan**

## Purpose:
The purpose of this penetration test is to evaluate the security and functionality of the **Booking System – User Registration & Login components**, identify vulnerabilities, confirm whether previously reported issues were fixed in Phase 2, and validate the system using both manual and automated testing (OWASP ZAP).

## Scope:
### Tested components:
- Registration page  
- Login page (Phase 2)  
- Input validation (email, password, birthdate)  
- Server error handling  
- XSS, SQLi, HTML injection, overflow tests  
- CSRF protection  
- Password policy  
- Session behavior  
- OWASP ZAP automated scans (Round 1, Round 2, Round 3)

### Exclusions:
- Admin panel  
- Reservation logic  
- Backend source code  
- MFA, email verification  
- API endpoints outside registration/login flow  

## Test approach:
**Gray-box testing** using a combination of:
- Manual input manipulation  
- Browser DevTools  
- Payload-based attacks  
- ZAP automated scanning  

## Test environment:
- OS: Windows 10/11  
- Browser: Chrome  
- Dockerized web application + PostgreSQL  
- OWASP ZAP 2.14  
- Tested URLs:  
  - Phase 1 → http://localhost:8000  
  - Phase 1 Part 2 → http://localhost:8001  
  - Phase 2 → http://localhost:8002  

## Testing dates:
- Phase 1: 28.11.2025 – 29.11.2025  
- Phase 2: 07.12.2025 – 08.12.2025  

## Assumptions:
- Only user registration & login flows are in scope  
- No admin credentials provided  
- Local environment may behave differently from production  

---

# 2️⃣ Executive Summary

### Phase 1 Summary:
Phase 1 contained several **high-risk vulnerabilities**, including SQL Injection, XSS, missing CSRF tokens, weak password policy, and missing security headers.

### Phase 2 Summary:
Phase 2 introduced **major improvements**:

✔ Input validation significantly strengthened  
✔ SQL Injection attempts are blocked  
✔ XSS payloads cannot bypass validation  
✔ Password minimum length enforced  
✔ Duplicate email check added  
✔ Email max length enforced (50 chars)  

However, **critical issues remain**:

❌ No CSRF protection  
❌ No rate limiting  
❌ No session cookies/security flags  
❌ Password policy is still weak (min length = 4)  

### Overall risk level: **Medium**

### Top 5 recommended actions (Phase 2):
1. **Implement CSRF protection** for all POST requests  
2. **Add rate limiting** to prevent brute-force attacks  
3. **Use secure session cookies** (HttpOnly, Secure, SameSite)  
4. **Strengthen password policy** (minimum 8–12 characters)  
5. **Add server-side sanitization** beyond HTML5 validation  

---

# 3️⃣ Severity Scale

| Severity | Description | Action |
|---------|-------------|--------|
| 🔴 High | Direct risk of system compromise | Fix immediately |
| 🟠 Medium | Security weakness requiring attention | Fix soon |
| 🟡 Low | Minor issues | Fix during improvements |
| 🔵 Info | Informational | Hardening recommended |

---

# 4️⃣ Findings

## 🔥 Phase 1 Findings (Original)

| ID | Severity | Finding | Description | Status (Phase 2) |
|----|----------|----------|-------------|------------------|
| F-01 | 🔴 High | SQL Injection allowed | Weak validation allowed SQL injection payloads | ✔ Fixed |
| F-02 | 🔴 High | XSS possible | HTML/JS accepted in input fields | ✔ Fixed |
| F-03 | 🟠 Medium | Missing CSRF token | No anti-CSRF protection in forms | ❌ Not Fixed |
| F-04 | 🟡 Low | Weak password policy | Password `123` accepted | ⚠ Partially Fixed (min length=4) |
| F-05 | 🟠 Medium | Missing security headers | Missing CSP, X-Frame-Options, etc. | ✔ Improved (CSP added) |

---

# 5️⃣ Phase 2 – Functional Test Results

| Test Case | Expected Result | Actual Result | Status |
|-----------|------------------|----------------|--------|
| Valid registration | Should succeed | ✔ Success message shown | PASS |
| Missing fields | Browser should stop submission | ✔ “Please fill out this field” | PASS |
| Invalid email | Browser should block | ✔ “Please include @” | PASS |
| Birthdate in future | Should throw error | ✔ “Birthdate cannot be in the future” | PASS |
| Weak password (3 chars) | Should fail | ✔ “Password must be at least 4 characters” | PASS |
| Duplicate email | Should not allow | ✔ “Email already exists” | PASS |
| Login with valid credentials | Should succeed | ✔ Login successful | PASS |

---

## 6️⃣ Phase 2 – Security Test Results (Table Summary)

| Test ID | Security Test | Payload / Method | Expected Behavior | Actual Result | Status |
|--------|----------------|------------------|-------------------|----------------|--------|
| S-01 | SQL Injection | `' OR 1=1 --` <br> `'; DROP TABLE users; --` | Input rejected, no SQL execution | Email format + length validation blocked input. No SQL errors. | ✔ Fixed |
| S-02 | XSS Injection | `"><script>alert(1)</script>` <br> `<img src=x onerror=alert(1)>` | Scripts should not execute | Browser validation blocked. No script executed. | ✔ Fixed |
| S-03 | HTML Injection | `<b>Hello</b>` | Tag should not render | Email validation prevented submission. | ✔ Fixed |
| S-04 | Overflow / Long Input | 200+ characters | Should limit length | Error: “Email must not exceed 50 characters.” | ✔ Fixed |
| S-05 | Weak Password Policy | Password: `123` | Weak password should be rejected | Minimum length now = 4 chars. Weak but enforced. | ⚠ Partially Fixed |
| S-06 | Rate Limiting | 6+ wrong login attempts | Should block or slow down | No lockout, unlimited attempts. | ❌ Not Fixed |
| S-07 | Session Cookie Security | Checked cookies after login | Cookies should have `HttpOnly`, `Secure`, `SameSite` | No cookies set at all. | ⚠ Not Implemented |
| S-08 | CSRF Protection | Checked HTML + ZAP alerts | Forms should include CSRF tokens | No CSRF token found. ZAP confirms absence. | ❌ Not Fixed |


## 7️⃣ OWASP ZAP – Round 3 Summary (Table Format)

| Alert ID | Severity | Issue Detected | Description | Evidence / Notes | Status |
|---------|----------|----------------|-------------|------------------|--------|
| Z-01 | 🟠 Medium | Absence of Anti-CSRF Tokens | Forms are missing CSRF tokens, making them vulnerable to CSRF attacks | ZAP flagged all POST forms | ❌ Not Fixed |
| Z-02 | 🔵 Info | Authentication Request Identified | ZAP detected login-like behavior; not a vulnerability | Informational only | ✔ No Action Needed |
| Z-03 | 🔵 Info | User-Agent Fuzzer Alerts (24) | ZAP fuzzed User-Agent header; minor inconsistencies found | No security impact | ✔ No Action Needed |
| Z-04 | 🟢 Improvement | CSP Header Present | CSP header now included in responses | ZAP confirms presence | ✔ Fixed |
| Z-05 | 🟢 Improvement | X-Frame-Options Present | Prevents clickjacking by blocking iframe embedding | ZAP confirms presence | ✔ Fixed |
| Z-06 | 🟢 Improvement | No SQL Injection Vectors Found | SQLi payloads did not trigger warnings | Based on ZAP passive scan | ✔ Fixed |
| Z-07 | 🟢 Improvement | No Reflected XSS Found | ZAP performed XSS checks; no reflection detected | Passed all tests | ✔ Fixed |


## Improvements from earlier phases:

✔ CSP header implemented

✔ X-Frame-Options header added

✔ No SQL Injection vectors detected

✔ No reflected XSS detected


## 8️⃣ Conclusion

Phase 2 demonstrates significant progress in input validation, error handling, and security hardening.
However, several key issues remain unresolved:

❌ CSRF protection still missing

❌ No rate limiting on login

❌ No session cookies / missing security flags

⚠ Password policy still weak

## Final Overall Risk: Medium

The application is improved but not yet production-ready.

## 9️⃣ Attachments

zap_report_round1.md

zap_report_round2.md

zap_report_round3.md

