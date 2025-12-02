
# OWASP ZAP Scan Report — Round 1  
**Application:** Booking System Register page only — Phase 1, Part 1  
**Target URL:** http://localhost:8000  
**Scan Mode:** Automated Scan (Spider + Passive Scan)  
**Date:** *28.11.2025*  
**Tester:** *Md Rasedul Islam*

---

# 1. Executive Summary

ZAP automated scan identified several missing security headers and potential vulnerabilities in the Booking System's registration page.  
These findings indicate weak security configuration and lack of protection against common attacks such as CSRF, clickjacking, and content injection.

Overall risk rating: **Medium–High**

---

# 2. Alerts Summary

| Severity | Alert Name | Instances |
|---------|------------|-----------|
| 🔴 High | None | 0 |
| 🟠 Medium | Absence of Anti-CSRF Tokens | 6 |
| 🟠 Medium | Content Security Policy (CSP) Header Not Set | 3 |
| 🟠 Medium | Format String Error | 1 |
| 🟠 Medium | Missing Anti-clickjacking Header | 3 |
| 🟡 Low | X-Content-Type-Options Header Missing | 6 |
| 🔵 Info | User Agent Fuzzer | 12 |

---

# 3. Detailed Findings

---

## 🟠 3.1 Absence of Anti-CSRF Tokens  
**Risk:** Medium  
**Description:**  
The registration form does not include any CSRF token. This allows attackers to make users submit unwanted form requests.

**Evidence:**  
All POST requests lack hidden CSRF fields or CSRF cookies.

**Recommendation:**  
Implement CSRF tokens for all forms.

---

## 🟠 3.2 Content Security Policy (CSP) Header Not Set  
**Risk:** Medium  
**Description:**  
The server does not send a Content Security Policy header. This increases risk of XSS and data injection attacks.

**Evidence:**  
Missing header in HTTP responses:  
`Content-Security-Policy`

**Recommendation:**  
Add CSP header blocking inline scripts.

---

## 🟠 3.3 Format String Error  
**Risk:** Medium  
**Description:**  
A format string issue was detected which may allow unintended formatting or injection in output.

**Recommendation:**  
Review string format operations to prevent misuse.

---

## 🟠 3.4 Missing Anti-Clickjacking Header  
**Risk:** Medium  
**Description:**  
The app can be embedded inside an iframe, leaving it open to clickjacking attacks.

**Evidence:**  
Missing header:  
`X-Frame-Options: DENY`

**Recommendation:**  
Add `X-Frame-Options` or `frame-ancestors` in CSP.

---

## 🟡 3.5 X-Content-Type-Options Header Missing  
**Risk:** Low  
**Description:**  
Without this header, browsers may MIME-sniff content incorrectly, exposing users to attacks.

**Recommendation:**  
Add header:  
`X-Content-Type-Options: nosniff`

---

## 🔵 3.6 User Agent Fuzzer  
**Risk:** Informational  
**Description:**  
ZAP sent various fuzzed User-Agent headers. Responses varied but no critical issues detected.

**Recommendation:**  
No immediate action needed.

---

# 4. Overall Assessment

The application lacks basic security headers and protections, making it vulnerable to multiple classes of attacks such as:

- CSRF  
- XSS injection  
- Clickjacking  
- MIME-type attacks  

These issues should be fixed before deploying the next version.

---

# 5. Attachments

ZAP raw report and exported Markdown attached in project folder:
- zap_report_round1.md

