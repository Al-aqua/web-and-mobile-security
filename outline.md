# Web and Mobile Security Course Outline

**10 Chapters | 20 Sessions (Theory + Practical Labs)**

---

## **Chapter 1: Introduction to Application Security Fundamentals**

**Theory Lecture :**

- Introduction to CIA Triad (Confidentiality, Integrity, Availability)
- OWASP Overview and Mission
- Web vs Mobile Security Landscape
- Common Attack Vectors and Threat Landscape
- Security in SDLC (Secure Development Lifecycle)

**Practical Lab :**

- Installing and configuring DVWA (Damn Vulnerable Web Application)
- Configuring browser proxy settings for Burp Suite
- Capturing and analyzing HTTP requests with Burp Suite Proxy
- Basic reconnaissance using browser developer tools (Network tab, Console)

---

## **Chapter 2: OWASP Top 10 Web - Part 1 (A01-A03)**

**Theory Lecture :**

- **A01:2025 - Broken Access Control**: Vertical/Horizontal privilege escalation, IDOR
- **A02:2025 - Security Misconfiguration**: Default configs, unnecessary features, verbose errors
- **A03:2025 - Software Supply Chain Failures**: Dependency vulnerabilities, SBOMs

**Practical Lab :**

- Exploiting IDOR vulnerabilities in DVWA (user profile access)
- Testing horizontal and vertical privilege escalation
- Identifying missing access controls using Burp Suite
- Quick review: Inspecting security headers with browser tools

---

## **Chapter 3: OWASP Top 10 Web - Part 2 (A04-A06)**

**Theory Lecture :**

- **A04:2025 - Cryptographic Failures**: Weak algorithms, improper key management
- **A05:2025 - Injection**: SQL, NoSQL, Command, LDAP injection
- **A06:2025 - Insecure Design**: Threat modeling, secure design patterns

**Practical Lab :**

- Manual SQL Injection attacks in DVWA (authentication bypass, data extraction)
- Using sqlmap for automated SQL injection testing
- Identifying injection points using Burp Suite Repeater
- Testing different SQL injection techniques (UNION, blind, error-based)

---

## **Chapter 4: OWASP Top 10 Web - Part 3 (A07-A10)**

**Theory Lecture :**

- **A07:2025 - Authentication Failures**: Session fixation, credential stuffing
- **A08:2025 - Software/Data Integrity Failures**: Insecure deserialization, unsigned code
- **A09:2025 - Logging & Alerting Failures**: Insufficient monitoring
- **A10:2025 - Mishandling of Exceptional Conditions**: Error handling issues

**Practical Lab :**

- Brute-forcing authentication with Burp Suite Intruder
- Session hijacking and cookie manipulation techniques
- Testing for session fixation vulnerabilities
- Analyzing error messages for sensitive information disclosure

---

## **Chapter 5: Static and Dynamic Code Analysis**

**Theory Lecture :**

- SAST vs DAST vs IAST comparison
- Common vulnerability patterns in code
- Secure code review checklist (OWASP Code Review Guide)
- CI/CD pipeline security integration

**Practical Lab :**

- Static analysis using Semgrep on vulnerable code samples
- Running automated OWASP ZAP scans against DVWA
- Manual code review exercise identifying common vulnerabilities
- Comparing SAST vs DAST findings on the same application

---

## **Chapter 6: Advanced Web Security Topics**

**Theory Lecture :**

- Cross-Site Scripting (XSS): Stored, Reflected, DOM-based
- Cross-Site Request Forgery (CSRF)
- XML External Entity (XXE) attacks
- Server-Side Request Forgery (SSRF) - now part of A01:2025

**Practical Lab :**

- Exploiting Reflected XSS in DVWA
- Exploiting Stored XSS and demonstrating impact
- Testing and bypassing CSRF protections
- Basic XSS filter bypass techniques

---

## **Chapter 7: Business Logic Vulnerabilities**

**Theory Lecture :**

- Understanding business logic flaws vs technical vulnerabilities
- OWASP Business Logic Testing methodology
- Common patterns: Price manipulation, workflow bypass, race conditions
- Excessive trust in client-side controls
- Time-of-check to time-of-use (TOCTOU) vulnerabilities
- Account enumeration and abuse of functionality

**Practical Lab :**

- Price manipulation in vulnerable e-commerce application
- Race condition exploitation using Burp Suite Turbo Intruder
- Bypassing multi-step workflows (payment process manipulation)
- Client-side validation bypass and parameter tampering

---

## **Chapter 8: API Security**

**Theory Lecture :**

- REST vs GraphQL vs SOAP security
- OWASP API Security Top 10
- API authentication mechanisms (OAuth 2.0, JWT, API Keys)
- Rate limiting and abuse prevention

**Practical Lab :**

- Testing APIs with Postman/cURL
- JWT manipulation and attacks
- Mass assignment vulnerabilities
- API fuzzing with tools like ffuf or Arjun

---

## **Chapter 9: Mobile Application Security (Android, iOS & Cross-Platform)**

**Theory Lecture :**

- Android & iOS architecture and security models
- OWASP MASVS (Mobile Application Security Verification Standard)
- Mobile OWASP Top 10
- Platform-specific vulnerabilities (insecure storage, improper platform usage)
- React Native and Flutter security considerations
- Certificate pinning and mobile DevSecOps

**Practical Lab - Android-Focused:**

- Static analysis of Android APK using JADX (decompiling and code review)
- Extracting and analyzing AndroidManifest.xml for security issues
- Intercepting mobile app traffic with Burp Suite (certificate pinning bypass intro)
- Identifying insecure data storage in decompiled code

---

## **Chapter 10: Security Assessment & Remediation**

**Theory Lecture :**

- Penetration testing methodology (PTES, OWASP WSTG)
- Vulnerability assessment vs penetration testing
- Report writing and CVSS scoring
- Remediation prioritization and secure coding practices
- Compliance frameworks (PCI-DSS, GDPR, HIPAA implications)

**Practical Lab :**

- Guided security assessment of a sample web application
- Documenting findings with CVSS scoring and severity ratings
- Writing an executive summary and technical findings report
- Prioritizing remediation recommendations based on risk

---

## **Additional Resources for Students:**

### **Tools to Install:**

- Burp Suite Professional (or Community Edition)
- OWASP ZAP
- Nmap
- Metasploit Framework
- Frida
- MobSF
- JADX
- Postman
- Docker (for vulnerable apps)

### **Vulnerable Applications for Practice:**

- **Web**: DVWA, WebGoat, Juice Shop, bWAPP
- **Mobile**: DIVA (Android), MSTG Hacking Playground
- **API**: crAPI, VAmPI

### **Reference Materials:**

- OWASP Top 10 2025 Documentation
- OWASP MASVS & MASTG (Mobile Testing Guide)
- OWASP Web Security Testing Guide (WSTG)
- OWASP Cheat Sheet Series
