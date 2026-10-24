# DVWA Penetration Testing Report

Vulnerability assessment and penetration testing (VAPT) project carried out on the **Damn Vulnerable Web Application (DVWA)** as part of the CyberSNova Cyber Security Internship.

## Author

| Field | Details |
|---|---|
| Name | Mallampalli Kumar Siva Sai |
| Email | kumarsivasai16@gmail.com |
| Reference No. | CSN-OC-OL-26266 |
| Registration No. | CSN-OC-2026-266 |
| Domain | Cyber Security |
| Project Level | Advanced |

## Project Statement

This project is a hands-on penetration test of DVWA, an intentionally insecure PHP/MySQL application built for security training. The goal is to build an isolated lab, install and configure DVWA, exploit each of its vulnerability modules at increasing security levels (Low, Medium, High), and document every step, finding, impact and fix in a professional report, following OWASP-style testing practice.

## Objectives

1. Set up a safe, isolated lab environment and install DVWA correctly.
2. Identify and demonstrate common web application vulnerabilities across the DVWA modules.
3. Compare how each vulnerability behaves at the Low, Medium and High levels.
4. Explain the impact of each finding and map it to the OWASP Top 10.
5. Rate each finding by severity using a defined risk methodology.
6. Provide clear remediation and secure coding recommendations.
7. Build practical skills in documenting and reporting security findings professionally.

## Rules of Engagement

- All testing is performed **only** against the locally hosted DVWA instance inside an isolated virtual lab.
- The lab runs on a Host-only VirtualBox network; DVWA is reachable only at `127.0.0.1` and is never exposed to any external network.
- No denial-of-service testing and no attacks on the host operating system outside the lab.
- No social engineering against any real website or person.
- Default/lab-only credentials (e.g. `kali`/`kali`) are used strictly inside the lab and never reused on a real system.
- A VM snapshot is taken before testing so the environment can be restored if anything breaks.
- Every finding is reproduced and recorded with evidence (screenshots, requests/responses) before being written up.
- Techniques and payloads in this report are documented for learning purposes only and must not be used against any system without the explicit, written authorization of its owner.

## Scope

**In scope**
- A locally hosted DVWA instance, version 2.5, running at `127.0.0.1` on a single Kali Linux 2025.1c VM.
- All DVWA modules: Brute Force, Command Injection, CSRF, File Inclusion, File Upload, Insecure CAPTCHA, SQL Injection, SQL Injection (Blind), Weak Session IDs, XSS (Reflected, Stored, DOM-based), CSP Bypass, API Testing.
- Security levels Low, Medium and High, tested to compare how defenses differ.
- The supporting components of the lab (Apache, PHP, MariaDB) only as they relate to DVWA.

**Out of scope**
- Any system, network or website other than the isolated lab.
- Denial-of-service testing and attacks on the host operating system outside the lab.
- Social engineering attacks on websites.

## Testing Methodology

A structured, manual-first approach based on the **OWASP Web Security Testing Guide (WSTG)**, following these phases:

| # | Phase | Description |
|---|---|---|
| 1 | Planning and Setup | Defined scope, prepared the virtual lab, installed and configured DVWA. |
| 2 | Reconnaissance | Reviewed each module, its input fields, parameters and normal behavior. |
| 3 | Vulnerability Identification | Tested inputs manually and with tools to find weak validation, missing controls and unsafe data handling. |
| 4 | Exploitation | Confirmed each weakness in a controlled way and recorded the result as proof of concept. |
| 5 | Impact Analysis | Assessed what an attacker could achieve and rated the severity. |
| 6 | Remediation | Identified the root cause and correct fix, and checked how the Impossible level handles it. |
| 7 | Reporting | Documented findings with screenshots, evidence and recommendations. |

Each module is tested first at **Low** to establish the baseline weakness, then retested at **Medium** and **High** to see how defenses change and where they can still be bypassed. Findings are rated Critical, High, Medium or Low based on ease of exploitation and potential damage (see Risk Rating Methodology in the full report).

## Tools and Environment

| # | Component | Details |
|---|---|---|
| 1 | Host Operating System | Windows 11 |
| 2 | Virtualization Platform | Oracle VirtualBox |
| 3 | Attacker Machine | Kali Linux 2025.1c |
| 4 | Target Machine | Same Kali Linux VM |
| 5 | Web Server | Apache |
| 6 | Server-Side Language | PHP 8.4 |
| 7 | Database | MariaDB |
| 8 | Target Application | DVWA version 2.5 |
| 9 | Network Mode | Host-only Adapter |

| # | Tool | Purpose |
|---|---|---|
| 1 | Web Browser (Firefox / Chrome) | Accessing DVWA and testing each module |
| 2 | Browser Developer Tools | Inspecting requests, responses, cookies and page source |
| 3 | Burp Suite Community Edition | Intercepting and modifying HTTP requests (Proxy, Intruder, Repeater) |
| 4 | sqlmap | Confirming and analyzing SQL Injection |
| 5 | Netcat | Catching reverse shells from exploited modules |
| 6 | Python HTTP Server | Hosting payloads for remote file inclusion and file-upload tests |
| 7 | Text Editor (Mousepad) | Viewing and editing DVWA configuration and source files |
| 8 | Screenshot Tool | Capturing evidence for the report |

## Lab Tracking

| Item | Status |
|---|---|
| Hypervisor | Oracle VirtualBox |
| VM | Kali Linux 2025.1c (single VM — acts as both attacker and target) |
| DVWA location | `/var/www/html/DVWA` |
| Access URL | `http://127.0.0.1/DVWA/login.php` |
| Network isolation | Host-only Adapter; never exposed beyond `127.0.0.1` |
| Pre-test snapshot | Taken before testing began, to allow restoring the VM if needed |
| Security levels covered | Low → Medium → High, tracked per module in the results tables |
| Setup verification | All DVWA setup-page checks confirmed green before testing started |

Each module's results are tracked in a **Low / Medium / High** table recording whether the attack was **Exploited**, **Partially** exploited (worked only with a bypass), or **Blocked**, so that progress and outstanding bypasses can be reviewed at a glance across the whole assessment.

## Evidence Guidelines

Rules for capturing and referencing evidence, so every finding can be reproduced and checked:

- **Naming convention:** `Fig <module>.<step>` (e.g. `Fig 7.1.1`), numbered in the order the step appears, per module and per security level. A sub-step shares the same number with a letter suffix (`Fig 7.1.1b`), never a duplicate number.
- **What to capture for every step:**
  - The module page with the DVWA **security level visible** (Low/Medium/High).
  - The **source code view**, where relevant, showing the validation or filter being tested.
  - The **exact payload or request sent**, ideally captured in Burp Suite (Proxy/Repeater) rather than just typed in the URL bar.
  - The **resulting response** — page content, error message, extracted data, or shell/connection obtained.
- **Caption every screenshot** with what it shows and the result (e.g. "Login attempt with kali/kali succeeding"), not just the figure number.
- **Label the result honestly:** if a bypass attempt failed, capture and caption that too — a blocked attempt is still evidence, and the module's Low/Medium/High results table must match what the screenshots actually show.
- **Placement:** keep evidence inside the report next to the step that produced it (under each module's **Steps Performed** and **Evidence** subsections in Section 7), not in a separate appendix, so a reader never has to flip back and forth.
- **Redact nothing that matters:** credentials used are lab-only (`kali`/`kali`), so they do not need blurring, but blur or crop anything unrelated to the test (e.g. unrelated terminal history, personal files).
- **Before submitting:** confirm every `Fig x.y` referenced in the text actually has a matching image, and every image has a caption — no orphaned references or uncaptioned screenshots.

## Report Structure

1. Executive Summary
2. Scope & Objectives
3. Methodology
4. Tools & Environment
5. Lab Setup & Environment Configuration
6. Summary of Findings (Severity Matrix)
7. Detailed Findings (one subsection per module, each with description, steps, evidence, impact, risk rating and remediation)
8. Risk Rating Methodology
9. Conclusion & Recommendations
10. Disclaimer

## Modules Tested

| Ref. | Module | OWASP Top 10 (2021) | Severity |
|---|---|---|---|
| 7.1 | Brute Force | A07 – Identification and Authentication Failures | High |
| 7.2 | Command Injection | A03 – Injection | Critical |
| 7.3 | Cross-Site Request Forgery (CSRF) | A01 – Broken Access Control | Medium |
| 7.4 | File Inclusion (LFI/RFI) | A03 – Injection | High |
| 7.5 | File Upload | A04 – Insecure Design | High |
| 7.6 | Insecure CAPTCHA | A07 – Identification and Authentication Failures | Medium |
| 7.7 | SQL Injection | A03 – Injection | Critical |
| 7.8 | SQL Injection (Blind) | A03 – Injection | High |
| 7.9 | Weak Session IDs / Session Management | A07 – Identification and Authentication Failures | High |
| 7.10 | Cross-Site Scripting (XSS), Reflected | A03 – Injection | Medium |
| 7.11 | Cross-Site Scripting (XSS), Stored | A03 – Injection | High |
| 7.12 | Cross-Site Scripting (XSS), DOM-based | A03 – Injection | Medium |
| 7.13 | Content Security Policy (CSP) Bypass | A05 – Security Misconfiguration | Medium |
| 7.14 | API Testing | A01/A03 | High |

**14 modules**, **2 Critical**, **7 High**, **5 Medium**, **0 Low** findings at the Low security level.

## Key Findings

The most serious findings were **SQL Injection** and **Command Injection** (both Critical), which could lead to full compromise of the database or server. At the Low security level every module was exploitable, as expected for an intentionally vulnerable application. Testing at Medium and High showed that partial protections — simple string filters, client-side checks, header checks — could often be bypassed with a different technique, while server-side validation, parameterized queries and values the client could not control (anti-CSRF tokens, output encoding) were the most effective defenses.

## Disclaimer

All testing was carried out exclusively against a locally hosted, intentionally vulnerable DVWA instance in an isolated lab not exposed to the internet or any production system. The techniques and payloads in this report are for learning purposes only and must never be used against a system without the explicit, written authorization of its owner.