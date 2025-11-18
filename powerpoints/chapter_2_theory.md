---
marp: true
theme: default
paginate: true
header: 'Chapter 2: OWASP Top 10 Web - Part 1 (A01-A03)'
footer: 'Web and Mobile Security Course'
---

<!-- _class: lead -->
# Chapter 2
## OWASP Top 10 Web - Part 1 (A01-A03)

**Web and Mobile Security Course**

---

# Learning Objectives

By the end of this lecture, you will be able to:

- Understand Broken Access Control vulnerabilities and impact
- Identify Security Misconfiguration issues in applications
- Recognize Software Supply Chain risks
- Recognize real-world examples of each vulnerability
- Know how to test for and prevent these issues

---

<!-- _class: lead -->
# Part 1: A01:2025 Broken Access Control
## The #1 Web Application Vulnerability

---

# What is Broken Access Control?

**Definition:** Access control enforces whether users can act on requests. Failures lead to unauthorized information disclosure, modification, or destruction of data.

**Impact:** Most critical vulnerability in OWASP Top 10 2025

**Key Issues:**
- Users can act outside intended permissions
- Attackers bypass authorization checks
- Access to protected resources without proper authentication

---

# Types of Broken Access Control (1/2)

**1. Insecure Direct Object Reference (IDOR)**
   - Accessing resources by manipulating object IDs
   - Example: `/api/users/123/profile` → change to `/api/users/124/profile`
   - Attacker accesses another user's data

**2. Horizontal Privilege Escalation**
   - User accesses data/functions at same privilege level
   - Example: User A views User B's order history
   - Same role, different data scope

---

# Types of Broken Access Control (2/2)

**3. Vertical Privilege Escalation**
   - User gains higher privileges than intended
   - Example: Regular user becomes admin
   - Can occur through parameter manipulation

**4. Missing Access Controls**
   - No checks on sensitive operations
   - Example: Admin functions accessible to anyone
   - Unprotected API endpoints

---

# Real-World Examples (1/2)

**Example 1: Facebook IDOR (2018)**
- Users could view other users' private information
- Simply changing user ID in URL revealed data
- Fixed after public disclosure

**Example 2: Equifax Data Exposure (2017)**
- Direct access to sensitive database without authentication
- Millions of personal records exposed
- Led to major regulatory actions

---

# Real-World Examples (2/2)

**Example 3: Tesla API Vulnerability (2021)**
- Attackers accessed other users' Tesla vehicle data
- Could see location, charging status, personal information
- Exploited IDOR in API endpoints

**Example 4: LinkedIn (Ongoing)**
- Data scraping via IDOR vulnerabilities
- Accessing profiles by ID manipulation

---

# Common Attack Scenarios (1/2)

**Scenario 1: Bank Account Access**
```
Legitimate: /mybank.com/account/view?id=1001
Attack: /mybank.com/account/view?id=1002
Result: Access to another account's balance and transactions
```

**Scenario 2: Admin Panel Access**
```
Legitimate user at: /app/dashboard
Admin functions at: /app/admin (no access check)
Attacker directly accesses: /app/admin
Result: Full administrative control
```

---

# Common Attack Scenarios (2/2)

**Scenario 3: File Download by ID**
```
Legitimate: /files/download?file_id=5
Attack: /files/download?file_id=1,2,3,4,6,7...
Result: Downloading all users' files
```

**Scenario 4: API Mass Assignment**
```
POST /api/user/profile
{"name": "John", "role": "admin", "credit": 9999}
Attacker sets own role and credit balance
```

---

# How to Test for IDOR (1/2)

**Manual Testing with Burp Suite:**
1. Intercept a request accessing your own resource
2. Change the ID parameter to another user's ID
3. Observe if you gain access
4. Test with sequential IDs, UUIDs, and special values

**Example:**
- User A: GET /api/users/12345/profile
- Change to: GET /api/users/12346/profile
- If you see User B's data → IDOR found

---

# How to Test for IDOR (2/2)

**Testing Horizontal Privilege Escalation:**
- Login with user account
- Try accessing admin endpoints
- Modify role/permission parameters
- Test access to restricted resources

**Testing Vertical Privilege Escalation:**
- Try changing user role in POST/PUT requests
- Manipulate token/session values
- Access admin functionality directly

---

# Prevention & Remediation (1/2)

**1. Implement Access Control Properly**
```
// Good: Check user authorization
if (user.id == request.user_id || user.isAdmin()) {
    return getUserData(user_id);
} else {
    return 403 Forbidden;
}
```

**2. Use Role-Based Access Control (RBAC)**
- Define clear roles (admin, user, moderator)
- Check permissions at every endpoint
- Principle of Least Privilege

---

# Prevention & Remediation (2/3)

**3. Enforce Checks Server-Side**
- Never trust client-side access controls
- Validate authorization on backend
- Use middleware to enforce access policies

**4. Use UUIDs Instead of Sequential IDs**
- Makes ID guessing harder
- Example: `/api/users/a1b2c3d4-e5f6/profile`
- Still check authorization, don't rely solely on obscurity

---

# Prevention & Remediation (3/3)

**5. Audit and Monitor**
- Log access attempts
- Alert on suspicious patterns
- Review access logs regularly

---

# OWASP ASVS - Access Control

**From OWASP Application Security Verification Standard:**

- V1: Access Control Verification
- V4: Access Control for Business Logic
- V13: API and Web Service Verification
- Key ASVS Requirements:
  - All access decisions centralized
  - Session tokens properly validated
  - Access based on least privilege
  - User cannot access other user's data

**Reference:** https://owasp.org/www-project-application-security-verification-standard/

---

<!-- _class: lead -->
# Part 2: A02:2025 Security Misconfiguration
## The Silent Killer

---

# What is Security Misconfiguration?

**Definition:** Missing appropriate security hardening across any part of the application stack, or insecure default configurations

**Impact:** Attackers find unpatched vulnerabilities, unnecessary features, or default credentials

**Scope:**
- Application frameworks
- Web servers
- Databases
- Cloud configurations
- Operating systems

---

# Types of Security Misconfiguration (1/2)

**1. Default Credentials**
   - Database: admin/admin, root/root
   - Admin panels: admin/admin
   - Services: default passwords never changed
   - Example: Apache Tomcat default manager credentials

**2. Unnecessary Features Enabled**
   - Debug mode in production
   - Verbose error messages
   - Diagnostic endpoints
   - Admin interfaces publicly accessible

---

# Types of Security Misconfiguration (2/2)

**3. Unpatched Systems**
   - Outdated software versions
   - Missing security patches
   - Known CVEs exploitable
   - Example: Apache Log4j (Log4Shell) CVE-2021-44228

**4. Missing Security Headers**
   - No HSTS (HTTP Strict Transport Security)
   - Missing CSP (Content Security Policy)
   - X-Frame-Options not set
   - No X-Content-Type-Options header

---

# Common Misconfiguration Issues (1/2)

**Issue 1: Verbose Error Messages**
```
Internal Server Error:
SQLException: Error in SQL query: SELECT * FROM users WHERE id=123
Database: PostgreSQL 9.6 on x86_64...
Stack trace: /app/database.php line 145
```
Attacker learns: Database type, framework, file paths

---

# Common Misconfiguration Issues (2/2)

**Issue 2: Directory Listing**
- Web server showing file directory contents
- `/backup/`, `/admin/`, `/config/` visible
- Attacker can find sensitive files

**Issue 3: Unnecessary Services Running**
- FTP server enabled alongside HTTPS
- Telnet accessible (unencrypted)
- Debug ports exposed

**Issue 4: Cloud Misconfigurations**
- S3 buckets publicly readable
- Database ports exposed to internet
- API keys in environment variables

---

# Real-World Misconfiguration Examples (1/2)

**Example 1: Capital One Data Breach (2019)**
- Misconfigured AWS firewall rules
- 100 million records exposed
- WAF misconfiguration allowed direct access
- Cost: $700 million settlement

**Example 2: Facebook Debug Info (2016)**
- Debug mode enabled in production
- Exposed sensitive system information
- Researchers disclosed vulnerability

---

# Real-World Misconfiguration Examples (2/2)

**Example 3: MongoDB Ransomware (2017)**
- Publicly exposed MongoDB instances
- Default credentials still active
- Attackers encrypted databases and demanded ransom
- Thousands of databases affected

**Example 4: Healthcare.gov (2013)**
- Debug information in error messages
- Code paths and database structure revealed
- Information disclosure leading to further attacks

---

# Testing for Misconfiguration (1/2)

**1. Port Scanning and Service Enumeration**
```bash
nmap -sV target.com  # Identify services and versions
```

**2. Check for Default Credentials**
- Try common combos: admin/admin, root/root
- Test databases, admin panels, devices
- Check for factory default settings

**3. Generate Error Messages**
- Input invalid data
- Observe error responses
- Look for stack traces and internal details

---

# Testing for Misconfiguration (2/2)

**4. Analyze Security Headers**
```bash
curl -I https://target.com
```
Look for: HSTS, CSP, X-Frame-Options, X-Content-Type-Options

**5. Check Directory Permissions**
- Try accessing common directories
- `/admin/`, `/backup/`, `/config/`, `/test/`
- Look for directory listing enabled

**6. Review Source Code**
- Check for debug code
- Hardcoded credentials
- Verbose logging in production

---

# Prevention & Remediation (1/3)

**1. Secure Configuration Management**
- Use infrastructure as code (Terraform, CloudFormation)
- Maintain configuration baselines
- Version control configurations
- Separate secrets from code (use vaults)

**2. Change All Default Credentials**
- Immediately on deployment
- For all systems: databases, admin panels, services
- Use strong, unique passwords
- Implement MFA where possible

---

# Prevention & Remediation (2/3)

**3. Implement Security Headers**
```
Strict-Transport-Security: max-age=31536000; includeSubDomains
Content-Security-Policy: default-src 'self'
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
```

**4. Disable Unnecessary Features**
- Debug mode OFF in production
- Verbose errors disabled
- Unnecessary services removed
- Admin interfaces protected or removed

---

# Prevention & Remediation (3/3)

**5. Keep Systems Patched**
- Regular security updates
- Automated patching where possible
- Vulnerability management program
- Patch management policy

**6. Security Scanning**
- Regular automated scanning
- Configuration audits
- Compliance checks
- Cloud security posture management (CSPM)

---

# Hardening Checklists (1/2)

**Web Server Hardening (Apache/Nginx):**
- Disable unnecessary modules
- Hide server version info (ServerTokens: Prod)
- Enable SSL/TLS with strong ciphers
- Set proper file permissions (not world-readable)
- Disable directory listing

---

# Hardening Checklists (2/2)

**Application Hardening:**
- Error handling should not expose internals
- Logging for security events (not debug)
- No hardcoded credentials
- Security headers implemented
- HTTPS enforced everywhere
- Regular dependency updates

---

# CIS Benchmarks and Standards

**Center for Internet Security (CIS) Benchmarks:**
- Configuration hardening standards for all major systems
- Free community editions available
- Industry-standard baselines
- Reference: https://www.cisecurity.org/

**Other Standards:**
- OWASP ASVS (Application Security Verification Standard)
- NIST Cybersecurity Framework
- PCI-DSS (for payment systems)

---

<!-- _class: lead -->
# Part 3: A03:2025 Software Supply Chain Failures
## Your Dependencies are Your Vulnerability

---

# What is Supply Chain Vulnerability?

**Definition:** Code and infrastructure failures related to missing or insecure dependencies, repositories, or CI/CD pipelines

**Impact:** Attackers compromise dependencies → affects all applications using them

**Scope:**
- Open source libraries
- Package managers (npm, pip, Maven)
- Third-party services
- CI/CD pipelines
- Build processes

---

# Supply Chain Attack Flow

**Attack Path:**
1. Attacker identifies popular open-source library
2. Gains access to maintainer account (phishing, credential stuffing)
3. Injects malicious code into library
4. Pushes update to package manager
5. Thousands of applications auto-update malicious code
6. Attackers execute payload in all affected systems

---

# Real-World Supply Chain Breaches (1/3)

**Example 1: Log4Shell (CVE-2021-44228)**
- Vulnerability in Apache Log4j library
- Affected: billions of Java applications
- Remote Code Execution (RCE)
- Extremely easy to exploit
- CVSS: 10.0 (Critical)

**Impact:**
- 60+ days before all patches applied
- Attackers compromised thousands of systems
- Ransomware attacks followed

---

# Real-World Supply Chain Breaches (2/3)

**Example 2: SolarWinds Orion Platform (2020)**
- Attackers compromised CI/CD build system
- Injected malware into legitimate updates
- 18,000 government and private organizations affected
- US Government agencies compromised
- Cost: Estimated $1+ billion

**Attribution:** Russian SVR intelligence agency

---

# Real-World Supply Chain Breaches (3/3)

**Example 3: Malicious npm Packages (Ongoing)**
- Typosquatting: packages named similar to popular ones
  - `react` vs `reac`, `lodash` vs `lo-dash`
- Stealing credentials and API keys
- Mining cryptocurrency on developer machines
- Installing backdoors

**Example 4: ua-parser-js (2021)**
- Downloaded 17+ million times weekly
- Compromised package contained password stealer
- Affected thousands of downstream projects

---

# Dependency Vulnerability Categories (1/2)

**1. Known Vulnerabilities (CVEs)**
- Published vulnerabilities in dependencies
- Public databases track them (NVD, CVE Details)
- Attackers scan for unpatched versions
- Tools can automatically detect them

**2. Abandoned Packages**
- Unmaintained libraries with no security updates
- Accumulate known vulnerabilities over time
- Example: packages abandoned by authors on npm

---

# Dependency Vulnerability Categories (2/2)

**3. Malicious Packages**
- Intentionally harmful code
- Harder to detect automatically
- Requires code review or reputation system
- Growing concern in npm and PyPI

**4. License Compliance Issues**
- GPL-licensed code in proprietary application
- License conflicts
- Legal liability exposure
- Not security but important for compliance

---

# Common Supply Chain Risks (1/2)

**Risk 1: Transitive Dependencies**
```
Your app → depends on → Library A
Library A → depends on → Library B
Library B → has → vulnerability

You're vulnerable even though you didn't directly use Library B
```

**Risk 2: Typosquatting**
```
You want: npm install lodash
Attacker creates: npm install lodash (similar but malicious)
Developer makes typo → installs malicious package
```

---

# Common Supply Chain Risks (2/2)

**Risk 3: Version Confusion**
- Different version numbering in private vs public repos
- npm confusion attack
- Attacker publishes higher version publicly
- System installs malicious public version

**Risk 4: CI/CD Compromise**
- Attacker gains access to build pipeline
- Modifies build process
- Injects malicious code into releases
- All users download compromised software

---

# Testing for Supply Chain Issues (1/2)

**1. Dependency Scanning**
```bash
npm audit                    # Check for known vulnerabilities
pip install safety; safety check
mvn dependency-check:check  # For Java
```

**2. Software Bill of Materials (SBOM)**
- Generate SBOM: `syft . -o json > sbom.json`
- Lists all components and versions
- Compare against known vulnerable versions
- Required by recent Executive Orders (US)

---

# Testing for Supply Chain Issues (2/2)

**3. Code Review of Dependencies**
- Sample code from key dependencies
- Look for suspicious patterns
- Check maintainer reputation
- Verify package signatures where available

**4. Monitor for Changes**
- Track dependency updates
- Review changelog before updating
- Use lock files to ensure reproducible builds
- Alert on unusual package activity

---

# Prevention & Remediation (1/2)

**1. Dependency Management**
- Use lock files (`package-lock.json`, `Pipfile.lock`)
- Pin critical dependencies to known-good versions
- Regular updates (balance security vs stability)
- Remove unused dependencies

**2. Scanning and Monitoring**
- Automated vulnerability scanning in CI/CD
- Tools: npm audit, Snyk, OWASP Dependency-Check
- Continuous monitoring for new CVEs
- Fail builds if critical vulnerabilities found

---

# Prevention & Remediation (2/2)

**3. Secure Development Practices**
- Code review for dependency changes
- Use private package repositories where needed
- Verify package signatures and checksums
- Trust only official sources

**4. Supply Chain Hardening**
- Restrict access to CI/CD pipelines
- Require multi-factor authentication
- Monitor for unauthorized changes
- Implement admission controls in deployment

---

# SBOM and Regulations

**Software Bill of Materials (SBOM):**
- Complete inventory of components
- Versions, licenses, vulnerabilities
- Increasingly required by compliance frameworks

**Recent Requirements:**
- US Executive Order 14028 (2021): SBOM required for federal contracts
- NIST SSDF (Secure Software Development Framework)
- EU Cyber Resilience Act
- SEC cybersecurity disclosure rules

---

# Tools for Supply Chain Security (1/2)

**Vulnerability Scanning:**
- npm audit - Node.js dependencies
- Snyk - Multi-language, focused on supply chain
- OWASP Dependency-Check - Java, .NET, Node.js, etc.
- Trivy - Container and dependency scanning

**SBOM Generation:**
- syft - CycloneDX and SPDX formats
- cdxgen - CycloneDX focused

---

# Tools for Supply Chain Security (2/2)

**CI/CD Security:**
- GitHub Supply Chain Security features
- GitLab Security scanning
- Jenkins security plugins
- HashiCorp Vault - secrets management

**Reference:**
https://owasp.org/www-community/attacks/Dependency_attack

---

<!-- _class: lead -->
# Comparison: A01, A02, A03

---

# A01 vs A02 vs A03 at a Glance

| Aspect | A01 Access Control | A02 Misconfiguration | A03 Supply Chain |
|--------|-------------------|----------------------|------------------|
| **When?** | Runtime access decisions | Deployment time | Build/dependency time |
| **Who?** | Application developers | Ops/DevOps teams | Dependency owners |
| **Fix Cost** | High (code changes) | Low (config changes) | Variable (may need forking) |
| **Detectability** | Testing/manual review | Scanning/audits | Automated tools |

---

# Vulnerability Severity Comparison

**A01: Broken Access Control**
- Affects: Confidentiality, Integrity, Availability
- Scope: All user data and functionality
- Detection: Sometimes easy (IDOR), sometimes hard (complex RBAC)

**A02: Security Misconfiguration**
- Affects: Mostly Confidentiality and Integrity
- Scope: System information and configuration data
- Detection: Relatively easy with scanners

**A03: Supply Chain Failures**
- Affects: Everything in the supply chain
- Scope: Potentially entire organization
- Detection: Hard without automated tools

---

<!-- _class: lead -->
# Key Takeaways

---

# Summary

**A01: Broken Access Control**
- Most critical vulnerability
- Users access unauthorized data/functionality
- Test with IDOR, privilege escalation
- Prevent with proper authorization checks

**A02: Security Misconfiguration**
- Missing hardening across the stack
- Default credentials, verbose errors, unpatched systems
- Prevent with security baselines and automation
- Regular scanning and audits essential

**A03: Supply Chain Failures**
- Compromised dependencies affect all users
- Requires dependency management and monitoring
- SBOM and vulnerability scanning critical
- CI/CD security paramount

---

# In the Lab (Chapter 2)

You will:
- Exploit IDOR vulnerabilities in DVWA
- Test horizontal and vertical privilege escalation
- Identify missing access controls
- Practice security header inspection
- Use Burp Suite for access control testing

---

# Looking Ahead

**Next Chapter (Chapter 3):**
- **A04:2025** - Cryptographic Failures
- **A05:2025** - Injection attacks
- **A06:2025** - Insecure Design

**Later Chapters:**
- Chapter 4: A07-A10 (remaining OWASP Top 10)
- Chapter 5: Static and Dynamic Analysis
- Chapter 6: Advanced Web Vulnerabilities (XSS, CSRF, XXE)

---

# Resources for Further Learning (1/2)

**OWASP Documentation:**
- https://owasp.org/www-project-top-ten/
- https://owasp.org/www-project-application-security-verification-standard/
- https://owasp.org/www-project-cheat-sheets/

**Supply Chain Security:**
- https://www.cisa.gov/news-events/news (Log4Shell, SolarWinds analysis)
- https://www.ntia.doc.gov/SBOM/ (SBOM guidance)

---

# Resources for Further Learning (2/2)

**Tools:**
- Burp Suite: https://portswigger.net/burp
- npm audit: https://docs.npmjs.com/cli/audit
- OWASP Dependency-Check: https://jeremylong.github.io/DependencyCheck/

---

<!-- _class: lead -->
# Questions?

**Next Class:** Chapter 2 Practical Lab
Exploiting and testing access control vulnerabilities

---

<!-- _class: lead -->
# Thank You!
