---
marp: true
theme: default
paginate: true
header: 'Chapter 4: OWASP Top 10 Web - Part 3 (A07-A10)'
footer: 'Web and Mobile Security Course'
---

<!-- _class: lead -->
# Chapter 4
## OWASP Top 10 Web - Part 3
### A07-A10:2025

**Web and Mobile Security Course**

---

# Learning Objectives

By the end of this lecture, you will be able to:

- Understand authentication failures and session management vulnerabilities
- Identify software and data integrity risks
- Recognize the importance of security logging and monitoring
- Explain proper error handling and exceptional conditions

---

# OWASP Top 10:2025 Coverage

**Previous Chapters:**
- Chapter 2: A01 (Broken Access Control), A02 (Security Misconfiguration), A03 (Supply Chain)
- Chapter 3: A04 (Cryptographic Failures), A05 (Injection), A06 (Insecure Design)

**This Chapter:**
- A07: Authentication Failures
- A08: Software/Data Integrity Failures
- A09: Logging & Alerting Failures
- A10: Mishandling of Exceptional Conditions

---

<!-- _class: lead -->
# Part 1: A07:2025
## Authentication Failures

---

# A07: Authentication Failures - Overview

**Previously known as:** Identification and Authentication Failures (2021)

**CWE Rank:** #22 CWEs mapped
**Incidence Rate:** 2.55% average
**Notable CWEs:**
- CWE-287: Improper Authentication
- CWE-384: Session Fixation
- CWE-307: Improper Restriction of Excessive Authentication Attempts

---

# What is Authentication?

**Authentication:** Verifying the identity of a user, process, or device

**Three Authentication Factors:**
1. **Something you know** - Password, PIN
2. **Something you have** - Phone, security token
3. **Something you are** - Biometrics (fingerprint, face)

**Multi-Factor Authentication (MFA):** Combining 2+ factors

---

# Common Authentication Vulnerabilities (1/2)

**1. Weak Password Policies**
- Allowing default passwords (admin/admin)
- No complexity requirements
- Well-known passwords (Password1, 123456)

**2. Credential Stuffing**
- Using lists of stolen username/password combinations
- Automated attacks against login forms
- Example: Passwords from LinkedIn breach used on banking sites

---

# Common Authentication Vulnerabilities (2/2)

**3. Brute Force Attacks**
- Automated password guessing
- No rate limiting or account lockout
- Dictionary attacks

**4. Weak Password Recovery**
- Security questions (mother's maiden name)
- Knowledge-based answers easily guessable
- Password reset tokens that don't expire

---

# Session Management Failures (1/2)

**What is a Session?**
- Temporary state maintained between client and server
- Identified by session ID (stored in cookie or token)

**Session Fixation:**
- Attacker sets victim's session ID before authentication
- Victim logs in with that session ID
- Attacker uses the same session ID to access victim's account

---

# Session Management Failures (2/2)

**Session Hijacking:**
- Stealing valid session IDs
- Methods: XSS, network sniffing, physical access

**Insufficient Session Expiration:**
- Sessions don't timeout after inactivity
- Sessions remain valid after logout
- Example: Public computer attack scenario

---

# Real-World Example: Session Fixation

```
1. Attacker visits site: www.bank.com/?sessionid=ATTACKER123
2. Attacker sends link to victim: www.bank.com/?sessionid=ATTACKER123
3. Victim clicks link and logs in
4. Application doesn't generate new session ID after login
5. Attacker uses sessionid=ATTACKER123 to access victim's account
```

**Impact:** Complete account takeover

---

# Attack Scenario: Credential Stuffing

**Scenario:**
- Application has no protection against automated attacks
- Attacker obtains 1 million username/password pairs from breach
- Uses automated tool to test credentials
- Application doesn't rate-limit or detect the attack
- 2% success rate = 20,000 compromised accounts

**Real Example:** 2023 GitHub credential stuffing attack

---

# How to Prevent Authentication Failures (1/2)

**1. Implement Multi-Factor Authentication**
- SMS codes, authenticator apps, hardware tokens
- Significantly reduces credential stuffing impact

**2. Enforce Strong Password Policies**
- Test against top 10,000 worst passwords list
- Follow NIST 800-63b guidelines

---

# How to Prevent Authentication Failures (2/2)

**3. Secure Session Management**
- Generate new session ID after login
- Use high-entropy random session IDs
- Don't expose session IDs in URLs
- Implement proper timeouts
- Invalidate sessions on logout

**4. Rate Limiting & Account Lockout**
- Limit failed login attempts
- Implement progressive delays
- Alert administrators on attacks

---

# NIST Password Guidelines Summary

**DO:**
- Minimum 8-character passwords (15+ for privileged accounts)
- Allow all ASCII characters and spaces
- Check against breach databases
- Use salted hashing (bcrypt, Argon2)

**DON'T:**
- Force periodic password changes
- Require arbitrary complexity rules
- Use password hints
- Truncate passwords

**Reference:** NIST SP 800-63B Section 5.1.1

---

<!-- _class: lead -->
# Part 2: A08:2025
## Software and Data Integrity Failures

---

# A08: Software/Data Integrity Failures - Overview

**New category in 2021, refined in 2025**

**CWE Rank:** #10 CWEs mapped
**Incidence Rate:** 2.05% average
**Impact:** Very high (7.94 weighted impact)

**Key Focus:** Assumptions about software updates and data without verifying integrity

---

# What is Integrity?

**Software Integrity:**
Ensuring code comes from expected source and hasn't been altered

**Data Integrity:**
Ensuring data hasn't been tampered with or modified

**The Problem:**
Applications trust updates, plugins, or data without verification

---

# Insecure Software Updates (1/2)

**The Vulnerability:**
- Applications download updates without signature verification
- No integrity checks on downloaded code
- Unsigned firmware and auto-updates

**Attack Vector:**
1. Attacker compromises update server or CDN
2. Injects malicious code into update
3. Application downloads and installs malicious update
4. Attacker gains widespread access

---

# Insecure Software Updates (2/2)

**Real-World Example: SolarWinds (2020)**
- Nation-state attackers compromised SolarWinds build system
- Injected malware into Orion software updates
- Distributed to 18,000+ organizations
- Affected ~100 high-value targets
- One of the largest supply chain attacks in history

**Impact:** Government agencies, Fortune 500 companies compromised

---

# Untrusted Dependencies (1/2)

**The Risk:**
- Applications use libraries from npm, Maven, PyPI
- Dependencies may contain vulnerabilities
- Malicious packages uploaded to repositories

**Supply Chain Attack Examples:**
- Typosquatting (event-stream vs. eventstream)
- Dependency confusion attacks
- Compromised maintainer accounts

---

# Untrusted Dependencies (2/2)

**Attack Scenario:**
```javascript
// package.json
{
  "dependencies": {
    "popular-library": "^2.0.0"  // Legitimate
  }
}
```

**Attacker Strategy:**
1. Creates similar package name: "popular-libary" (typo)
2. Adds malicious code
3. Developers accidentally install wrong package
4. Malicious code exfiltrates environment variables, API keys

---

# Insecure CI/CD Pipelines (1/2)

**Common Issues:**
- Insufficient access controls
- No code signing
- Secrets exposed in build logs
- Unvalidated build artifacts

**Attack Vector:**
- Attacker compromises CI/CD system
- Injects malicious code during build
- Malicious code deployed to production

---

# Insecure CI/CD Pipelines (2/2)

**Real-World Example: CodeCov Bash Uploader (2021)**
- Attackers modified Codecov's Bash Uploader script
- Script had access to environment variables and secrets
- Compromised script exfiltrated credentials
- Affected hundreds of customers

**Lesson:** Even security tools can be compromised!

---

# Insecure Deserialization (1/3)

**What is Serialization?**
Converting objects to byte streams for storage/transmission

**What is Deserialization?**
Converting byte streams back to objects

**The Vulnerability:**
Deserializing untrusted data can execute arbitrary code

---

# Insecure Deserialization (2/3)

**How It Works:**
1. Application serializes user state/session data
2. Sends serialized data to client
3. Client sends modified serialized data back
4. Application deserializes without validation
5. Attacker-controlled code executes

**Java Object Signature:** `rO0` in Base64

---

# Insecure Deserialization (3/3)

**Attack Example:**
```python
# Vulnerable Python code
import pickle
user_data = request.cookies.get('session')
session = pickle.loads(base64.b64decode(user_data))  # DANGER!
```

**Impact:**
- Remote code execution
- Authentication bypass
- Full system compromise

**Famous Tool:** ysoserial (Java deserialization exploits)

---

# How to Prevent Integrity Failures (1/2)

**1. Digital Signatures & Verification**
- Sign all software updates with private key
- Verify signatures before installation
- Use code signing certificates

**2. Secure Dependency Management**
- Use trusted repositories only
- Consider private/internal repositories
- Use tools: OWASP Dependency-Check, npm audit
- Generate and verify SBOMs (Software Bill of Materials)

---

# How to Prevent Integrity Failures (2/3)

**3. Secure CI/CD Practices**
- Implement pipeline segregation
- Use least privilege access
- Sign build artifacts
- Review code and configuration changes
- Audit all pipeline modifications

---

# How to Prevent Integrity Failures (3/3)

**4. Avoid Insecure Deserialization**
- Don't deserialize untrusted data
- Use JSON instead of native serialization
- Implement integrity checks (HMAC)
- Use language-specific safe alternatives

---

<!-- _class: lead -->
# Part 3: A09:2025
## Logging and Alerting Failures

---

# A09: Logging & Alerting Failures - Overview

**Previously:** Security Logging and Monitoring Failures

**CWE Rank:** #4 CWEs mapped
**Incidence Rate:** 6.51% average

**Key CWEs:**
- CWE-778: Insufficient Logging
- CWE-117: Improper Output Neutralization for Logs
- CWE-532: Insertion of Sensitive Information into Log File

---

# Why Logging Matters

**Critical for:**
- Detecting security incidents
- Forensic analysis after breach
- Compliance requirements
- Debugging and troubleshooting
- Accountability and audit trails

**Reality:** Most web attacks go undetected because applications don't log properly

---

# Common Logging Failures (1/2)

**1. Insufficient Logging**
- Login attempts not logged
- Failed access control checks ignored
- High-value transactions not audited
- Administrative actions not recorded

**2. Inadequate Log Messages**
- No context (user ID, IP, timestamp)
- Unclear error messages
- Missing security-relevant events

---

# Common Logging Failures (2/2)

**3. Local-Only Storage**
- Logs stored only on application server
- Attacker can delete logs after compromise
- No centralized log aggregation

**4. No Monitoring or Alerting**
- Logs generated but never reviewed
- No automated threat detection
- Security events don't trigger alerts
- No incident response process

---

# What Should Be Logged? (1/2)

**Authentication Events:**
- Successful/failed logins
- Password changes
- Account lockouts
- MFA usage

**Authorization Events:**
- Access control failures
- Privilege escalation attempts
- Permission changes

---

# What Should Be Logged? (2/2)

**Application Events:**
- Input validation failures
- High-value transactions
- Administrative actions
- System errors and exceptions

**Security Events:**
- Suspicious patterns (rapid requests)
- DAST/penetration test activity
- Unusual data access patterns

---

# What NOT to Log

**Sensitive Information:**
- Passwords or password hashes
- Session tokens or API keys
- Credit card numbers (PAN)
- Personally Identifiable Information (PII)
- Healthcare information (PHI)

**Legal Risk:** Logging sensitive data can create compliance issues (GDPR, CCPA, HIPAA)

---

# Log Injection Vulnerabilities

**The Problem:**
User input not sanitized before logging

**Attack Example:**
```python
# Vulnerable code
username = request.form['username']
logger.info(f"Login attempt: {username}")

# Attacker input: admin\n[ERROR] System compromised
# Resulting log:
# [INFO] Login attempt: admin
# [ERROR] System compromised
```

**Impact:** Log forgery, hiding malicious activity

---

# Real-World Example: Data Breach

**Scenario: Indian Airline Breach (2022)**
- Data breach affecting 10 years of passenger data
- Millions of passengers impacted
- Breach occurred at third-party cloud provider
- No monitoring detected the breach
- External party eventually notified airline
- Unknown how long breach was active

**Lesson:** Without monitoring, breaches go undetected for months/years

---

# Effective Logging Architecture (1/2)

**Log Collection:**
- Application logs
- Web server logs (access, error)
- Database logs
- Network logs

**Centralized Log Management:**
- SIEM (Security Information and Event Management)
- ELK Stack (Elasticsearch, Logstash, Kibana)
- Splunk, Datadog, etc.

---

# Effective Logging Architecture (2/2)

**Log Retention:**
- Sufficient time for forensics (typically 90+ days)
- Compliance requirements may mandate longer

---

# How to Prevent Logging Failures (1/2)

**1. Comprehensive Logging Strategy**
- Log all security-relevant events
- Include sufficient context
- Use consistent log format
- Follow OWASP Logging Cheat Sheet

**2. Secure Log Storage**
- Centralized log aggregation
- Append-only storage (prevent tampering)
- Encrypted logs at rest and in transit
- Access controls on log data

---

# How to Prevent Logging Failures (2/2)

**3. Active Monitoring & Alerting**
- Real-time threat detection
- Automated alerts for suspicious activity
- Security dashboards
- Regular log review

**4. Incident Response Plan**
- Documented procedures (NIST 800-61r2)
- Clear escalation paths
- Regular drills and testing
- Post-incident review process

---

<!-- _class: lead -->
# Part 4: A10:2025
## Mishandling of Exceptional Conditions

---

# A10: Mishandling of Exceptional Conditions

**New addition to OWASP Top 10:2025**

**Focus Areas:**
- Improper error handling
- Information disclosure through errors
- Inconsistent error messages
- Application stability issues

**Related to:** Security Logging (A09) but focused on error handling specifically

---

# What Are Exceptional Conditions?

**Exceptions/Errors:**
- Out of memory
- Null pointer exceptions
- Database unavailable
- Network timeouts
- File not found
- Invalid input
- System call failures

**Normal Operation:** Applications generate hundreds of error conditions

---

# The Security Problem (1/2)

**Improper Handling Leads To:**

1. **Information Disclosure**
   - Stack traces reveal code structure
   - Database errors expose schema
   - Paths reveal directory structure

2. **Inconsistent Behavior**
   - Different errors reveal system state
   - Attackers map application through errors

---

# The Security Problem (2/2)

**3. Fail-Open Vulnerabilities**
   - Error bypasses security check
   - Default allows access on failure

---

# Information Disclosure Examples (1/2)

**Stack Trace Exposure:**
```
Exception in thread "main" java.lang.NullPointerException
    at com.bank.account.AccountService.withdraw(AccountService.java:45)
    at com.bank.web.TransactionController.processTransaction(TransactionController.java:123)
    ...
```

**Reveals:**
- Programming language (Java)
- Internal code structure
- File paths and line numbers
- Framework details

---

# Information Disclosure Examples (2/2)

**Database Error:**
```sql
MySQL Error: Table 'users' doesn't exist in database 'production_db'
Query: SELECT * FROM users WHERE username='admin'
```

**Reveals:**
- Database type (MySQL)
- Database name (production_db)
- Table structure
- SQL query syntax
- Column names

---

# Error Message Inconsistencies

**Scenario: File Access**

**Attempting to access file that doesn't exist:**
```
Error: File not found
```

**Attempting to access restricted file:**
```
Error: Access denied
```

**Problem:** Attacker learns which files exist through error messages

**Better Approach:** Consistent message - "Unable to access file"

---

# Error Message Inconsistencies: Authentication

**Bad Practice:**
```
Login failed: Invalid username
Login failed: Invalid password
```

**Why it's bad:** Reveals which usernames exist (account enumeration)

**Good Practice:**
```
Login failed: Invalid username or password
```

**Same message for both cases**

---

# Fail-Open vs Fail-Closed (1/2)

**Fail-Open (Insecure):**
- Error in security check → Allow access
- Example: Authentication service unavailable → Let everyone in

**Fail-Closed (Secure):**
- Error in security check → Deny access
- Example: Authentication service unavailable → Deny access

**Security Principle:** Deny by default, allow explicitly

---

# Fail-Open vs Fail-Closed (2/2)

**Vulnerable Code Example:**
```python
def check_admin_access(user):
    try:
        role = database.get_user_role(user)
        return role == 'admin'
    except DatabaseError:
        return True  # FAIL-OPEN: Error grants access!

# Correct version:
    except DatabaseError:
        return False  # FAIL-CLOSED: Error denies access
```

---

# Resource Exhaustion Through Errors

**Problem:**
Unhandled errors can cause:
- Memory leaks
- File handle exhaustion
- Database connection pool depletion
- Application crashes

**Impact:**
Denial of Service to legitimate users

**Example:** Repeated errors from invalid input consume all database connections

---

# Real-World Attack Scenario

**Scenario: E-commerce Application**

1. Attacker submits malformed payment data
2. Application generates verbose error exposing payment gateway details
3. Attacker learns payment provider and API version
4. Searches for known vulnerabilities in that version
5. Crafts specific exploit based on error information
6. Bypasses payment processing

**Prevention:** Generic error messages to users, detailed logs for developers

---

# How to Prevent Exceptional Condition Mishandling (1/3)

**1. Comprehensive Error Handling**
- Catch all possible exceptions
- Handle errors gracefully
- Never expose internal details to users
- Use try-catch blocks appropriately

**2. Consistent Error Messages**
- Same message for similar error types
- Don't reveal system state
- User-friendly but not informative to attackers

---

# How to Prevent Exceptional Condition Mishandling (2/3)

**3. Separation of Error Messages**

**User-Facing:**
```
We're experiencing technical difficulties. 
Please try again later or contact support.
Reference ID: 7a8b3c2d
```

**Server-Side Log (with Reference ID):**
```
[ERROR] 7a8b3c2d - Database connection timeout
at PaymentService.processPayment() line 234
Connection string: jdbc:mysql://db.internal:3306/payments
```

---

# How to Prevent Exceptional Condition Mishandling (3/3)

**4. Fail-Closed Design**
- Deny access by default
- Security checks fail securely
- Test error paths thoroughly

**5. Error Budget & Monitoring**
- Track error rates
- Alert on unusual error patterns
- May indicate attack in progress

---

# Error Handling Best Practices (1/2)

**DO:**
- Log detailed errors server-side
- Show generic messages to users
- Test error handling paths
- Use centralized error handling
- Implement proper resource cleanup

---

# Error Handling Best Practices (2/2)

**DON'T:**
- Display stack traces to users
- Expose database errors
- Reveal file paths or system info
- Ignore errors silently
- Grant access on errors

---

<!-- _class: lead -->
# Key Takeaways

---

# Summary - A07: Authentication Failures

**Key Points:**
- Authentication verifies user identity
- Common issues: weak passwords, credential stuffing, brute force
- Session management is critical
- Session fixation and hijacking are real threats
- MFA significantly improves security
- Follow NIST password guidelines
- Proper session timeouts and invalidation essential

---

# Summary - A08: Software/Data Integrity

**Key Points:**
- Verify integrity of software updates and dependencies
- SolarWinds showed impact of supply chain attacks
- Insecure deserialization enables RCE
- Secure CI/CD pipelines prevent injection of malicious code
- Use digital signatures and code signing
- SBOM helps track dependencies
- Trust but verify all external code

---

# Summary - A09: Logging & Alerting

**Key Points:**
- Most attacks go undetected without proper logging
- Log all security-relevant events with context
- Don't log sensitive information (passwords, tokens)
- Centralized log management prevents tampering
- Active monitoring and alerting detect attacks
- Incident response plan essential
- Log injection is a real vulnerability
- Compliance requires logging

---

# Summary - A10: Exceptional Conditions

**Key Points:**
- Errors reveal sensitive information if not handled properly
- Stack traces and database errors expose internals
- Inconsistent errors enable reconnaissance
- Fail-closed, not fail-open
- Separate user messages from developer logs
- Handle all exceptions gracefully
- Test error paths thoroughly
- Resource exhaustion can cause DoS

---

# The Complete OWASP Top 10:2025

**A01** - Broken Access Control  
**A02** - Security Misconfiguration  
**A03** - Software Supply Chain Failures  
**A04** - Cryptographic Failures  
**A05** - Injection  
**A06** - Insecure Design  
**A07** - Authentication Failures ✓  
**A08** - Software/Data Integrity Failures ✓  
**A09** - Logging & Alerting Failures ✓  
**A10** - Mishandling of Exceptional Conditions ✓

---

# How These Vulnerabilities Interconnect

**Authentication (A07) ↔ Logging (A09)**
- Failed authentication attempts must be logged
- Without logs, can't detect credential stuffing

**Integrity (A08) ↔ Access Control (A01)**
- Tampered data can bypass access controls
- Insecure deserialization often leads to privilege escalation

**Error Handling (A10) ↔ All Categories**
- Errors expose all vulnerability types
- Poor error handling amplifies other risks

---

# Real-World Impact Statistics

**Authentication Attacks:**
- 80% of breaches involve stolen credentials (Verizon DBIR)
- Average credential stuffing attack: 1M+ login attempts

**Supply Chain:**
- 62% increase in supply chain attacks (2023)
- SolarWinds affected 18,000+ organizations

**Logging Failures:**
- Average time to detect breach: 207 days (IBM)
- 67% of organizations lack visibility into attacks

---

# Defense in Depth Strategy (1/2)

**Layer 1: Prevention**
- Strong authentication (MFA)
- Signed updates
- Proper error handling

**Layer 2: Detection**
- Comprehensive logging
- Active monitoring
- Anomaly detection

---

# Defense in Depth Strategy (2/2)

**Layer 3: Response**
- Incident response plan
- Forensic capabilities
- Recovery procedures

**All layers must work together!**

---

# Looking Ahead

**Next Chapters:**
- **Chapter 5:** Static and Dynamic Code Analysis (SAST/DAST)
- **Chapter 6:** Advanced Web Security (XSS, CSRF, XXE, SSRF)
- **Chapter 7:** Business Logic Vulnerabilities

**Next Lab:**
- Hands-on with Burp Suite Intruder for brute forcing
- Session manipulation techniques
- Cookie analysis and modification
- Error message analysis

---

# Resources for Further Learning (1/2)

**OWASP Resources:**
- OWASP Top 10:2025 - https://owasp.org/Top10/
- OWASP Authentication Cheat Sheet
- OWASP Logging Cheat Sheet
- OWASP Deserialization Cheat Sheet

**Standards:**
- NIST SP 800-63B (Digital Identity Guidelines)
- NIST SP 800-61r2 (Incident Handling)

---

# Resources for Further Learning (2/2)

**Tools:**
- Burp Suite, OWASP ZAP
- OWASP Dependency-Check
- ysoserial, Java Deserialization Scanner

---

# Practical Exercise Ideas

**Try This Week:**
1. Enable MFA on your important accounts
2. Check your password strength at haveibeenpwned.com
3. Review logging configuration of a sample application
4. Analyze error messages on websites you use
5. Set up ELK stack in Docker for log aggregation

**Remember:** Best way to learn security is to practice!

---

<!-- _class: lead -->
# Questions?

**Next Class:** Chapter 4 Practical Lab
Authentication attacks and session manipulation with Burp Suite

---

<!-- _class: lead -->
# Thank You!

**Stay Curious, Stay Secure**
