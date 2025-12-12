---
marp: true
theme: default
paginate: true
header: 'Chapter 5: Static and Dynamic Code Analysis'
footer: 'Web and Mobile Security Course'
---

<!-- _class: lead -->
# Chapter 5
## Static and Dynamic Code Analysis

**Web and Mobile Security Course**

---

# Learning Objectives

By the end of this lecture, you will be able to:

- Compare SAST, DAST, and IAST methodologies
- Identify common vulnerability patterns in code
- Apply secure code review principles
- Understand CI/CD security integration strategies
- Use automated security testing tools effectively

---

# Why Code Analysis Matters (1/2)

**The Reality:**
- 90% of security incidents exploit known vulnerabilities
- Average cost to fix bugs in production: 100x more than in development
- Most vulnerabilities are introduced during coding phase

**Security Shift-Left:**
- Finding issues earlier = cheaper fixes
- Automated testing = consistent security checks
- Integrated security = faster, secure releases

---

# Why Code Analysis Matters (2/2)

**Reference to Previous Chapters:**
- Chapter 2-4 covered OWASP Top 10 vulnerabilities
- **This chapter:** Tools and techniques to find them before production
- Chapter 10 will cover complete assessment methodology

**Key Principle:** Prevention is better (and cheaper) than cure

---

<!-- _class: lead -->
# Part 1: SAST vs DAST vs IAST
## Understanding Security Testing Methodologies

---

# The Three Testing Approaches

**SAST** - Static Application Security Testing
- **When:** During development (code-level)
- **Analogy:** Reading the blueprint before building

**DAST** - Dynamic Application Security Testing
- **When:** During runtime (application-level)
- **Analogy:** Testing a building by trying to break in

**IAST** - Interactive Application Security Testing
- **When:** During runtime with instrumentation
- **Analogy:** Building with sensors that detect weaknesses

---

# SAST: Static Application Security Testing (1/3)

**Definition:** Analyzing source code, bytecode, or binaries without executing the application

**How it works:**
- Scans code for known vulnerability patterns
- Analyzes control flow and data flow
- Checks against security rules and coding standards
- White-box testing (requires source code access)

---

# SAST: Static Application Security Testing (2/4)

**Strengths:**
- Finds issues early in SDLC
- Pinpoints exact code location (file, line number)
- No need for running application
- Comprehensive code coverage

---

# SAST: Static Application Security Testing (3/4)

**Weaknesses:**
- High false positive rate (20-40%)
- Cannot find runtime/configuration issues
- Misses business logic flaws
- Requires source code access

---

# SAST: Static Application Security Testing (4/5)

**What SAST Finds:**
- SQL injection vulnerabilities
- Hard-coded credentials
- Buffer overflows
- Cross-site scripting (XSS) patterns

---

# SAST: Static Application Security Testing (5/5)

**What SAST Also Finds:**
- Insecure cryptography usage
- Path traversal vulnerabilities

**What SAST Misses:**
- Authentication/authorization bypass
- Server misconfigurations
- Runtime environment issues

---

# DAST: Dynamic Application Security Testing (1/3)

**Definition:** Testing running applications from the outside by simulating attacks

**How it works:**
- Crawls/spiders the application
- Sends malicious payloads to inputs
- Observes responses and behaviors
- Black-box testing (no source code needed)
- Tests like a real attacker would

---

# DAST: Dynamic Application Security Testing (2/4)

**Strengths:**
- Tests actual runtime behavior
- Finds configuration issues
- Low false positive rate
- Language/technology agnostic

---

# DAST: Dynamic Application Security Testing (3/4)

**More Strengths:**
- Tests authentication/authorization
- Identifies server misconfigurations

**Weaknesses:**
- Cannot test all code paths
- Slower than SAST

---

# DAST: Dynamic Application Security Testing (4/4)

**More Weaknesses:**
- Requires deployed application
- No exact code location provided
- May miss complex vulnerabilities

---

# DAST: What It Finds (1/2)

**What DAST Finds:**
- Authentication bypass
- Session management issues
- Server misconfigurations
- SSL/TLS vulnerabilities

---

# DAST: What It Finds (2/2)

**More Findings:**
- Security header issues
- Runtime injection attacks

**Best for:**
- Pre-production testing
- Penetration testing simulation
- Compliance requirements

---

# IAST: Interactive Application Security Testing (1/2)

**Definition:** Combines SAST and DAST by instrumenting running applications with agents

**How it works:**
- Agents/sensors embedded in application
- Monitors code execution in real-time
- Tracks data flow during normal testing
- Gray-box testing (has internal visibility)

---

# IAST: Interactive Application Security Testing (2/3)

**Strengths:**
- Low false positive rate
- Identifies exact code location
- Tests during normal QA/functional testing
- Understands runtime context

---

# IAST: Interactive Application Security Testing (3/3)

**More Strengths:**
- Detects complex vulnerabilities

**Weaknesses:**
- Requires application modification
- Performance overhead
- Limited language support
- More complex setup

---

# SAST vs DAST vs IAST Comparison (1/2)

| Aspect | SAST | DAST | IAST |
|--------|------|------|------|
| **Testing Type** | White-box | Black-box | Gray-box |
| **Requires Code** | Yes | No | No |
| **When to Run** | Development | Pre-production | QA/Testing |
| **Speed** | Fast | Slow | Medium |

---

# SAST vs DAST vs IAST Comparison (2/2)

| Aspect | SAST | DAST | IAST |
|--------|------|------|------|
| **False Positives** | High | Low | Low |
| **Code Location** | Exact | No | Exact |
| **Coverage** | Comprehensive | Partial | Good |
| **Runtime Issues** | No | Yes | Yes |

**Key Insight:** Use multiple approaches for comprehensive security coverage!

---

# When to Use Each Approach (1/2)

**SAST:**
- Early in development (commit/PR stage)
- Code reviews
- Finding common coding errors
- Compliance requirements

---

# When to Use Each Approach (2/2)

**DAST:**
- Before production deployment
- Penetration testing
- Testing APIs and web apps
- External security audits

**IAST:**
- During QA testing phase
- When high accuracy needed

---

<!-- _class: lead -->
# Part 2: Common Vulnerability Patterns
## What to Look for in Code

---

# Vulnerability Pattern Categories (1/2)

**From Previous Chapters (OWASP Top 10):**
1. **Injection Flaws** (Chapter 3)
   - SQL, NoSQL, Command injection
2. **Broken Access Control** (Chapter 2)
   - IDOR, privilege escalation
3. **Cryptographic Failures** (Chapter 3)
   - Weak algorithms, key management

---

# Vulnerability Pattern Categories (2/2)

4. **Authentication Issues** (Chapter 4)
   - Session management, weak credentials
5. **Security Misconfiguration** (Chapter 2)
   - Default settings, verbose errors
6. **Insecure Design** (Chapter 3)
   - Business logic flaws

**This chapter:** How to detect these in code

---

# Pattern 1: SQL Injection (1/2)

**Vulnerable Code (Java):**
```java
String query = "SELECT * FROM users WHERE username='" 
               + username + "' AND password='" + password + "'";
Statement stmt = connection.createStatement();
ResultSet rs = stmt.executeQuery(query);
```

**Problem:** Direct string concatenation with user input

---

# Pattern 1: SQL Injection (2/2)

**Secure Code (Java):**
```java
String query = "SELECT * FROM users WHERE username=? AND password=?";
PreparedStatement stmt = connection.prepareStatement(query);
stmt.setString(1, username);
stmt.setString(2, password);
ResultSet rs = stmt.executeQuery();
```

**Fix:** Use parameterized queries (prepared statements)

---

# Pattern 2: Hard-coded Credentials (1/2)

**Vulnerable Code (Python):**
```python
# Bad practice - credentials in code
DB_USER = "admin"
DB_PASSWORD = "SuperSecret123!"
connection = mysql.connect(
    host="localhost",
    user=DB_USER,
    password=DB_PASSWORD
)
```

**Problem:** Credentials exposed in source code, version control

---

# Pattern 2: Hard-coded Credentials (2/2)

**Secure Code (Python):**
```python
import os

# Good practice - use environment variables
DB_USER = os.environ.get('DB_USER')
DB_PASSWORD = os.environ.get('DB_PASSWORD')
connection = mysql.connect(
    host=os.environ.get('DB_HOST'),
    user=DB_USER,
    password=DB_PASSWORD
)
```

**Fix:** Use environment variables or secret management systems

---

# Pattern 3: Path Traversal

**Vulnerable Code (Node.js):**
```javascript
app.get('/download', (req, res) => {
    const filename = req.query.file;
    res.sendFile('/uploads/' + filename);
});
// Attack: /download?file=../../etc/passwd
```

**Secure Code:**
```javascript
const path = require('path');
app.get('/download', (req, res) => {
    const filename = path.basename(req.query.file);
    const filepath = path.join(__dirname, 'uploads', filename);
    res.sendFile(filepath);
});
```

---

# Pattern 4: Insecure Deserialization (1/2)

**Vulnerable Code (Python):**
```python
import pickle

def load_session(session_cookie):
    # DANGER: Deserializing untrusted data
    session_data = pickle.loads(base64.b64decode(session_cookie))
    return session_data
```

**Problem:** pickle.loads can execute arbitrary code

---

# Pattern 4: Insecure Deserialization (2/2)

**Secure Alternative (Python):**
```python
import json
import hmac

def load_session(session_cookie):
    # Use JSON instead of pickle
    session_data = json.loads(base64.b64decode(session_cookie))
    # Verify HMAC signature
    if not verify_signature(session_data):
        raise SecurityException("Invalid session")
    return session_data
```

**Fix:** Use JSON; implement signature verification

---

# Pattern 5: XSS Vulnerabilities

**Vulnerable Code (JavaScript/React):**
```javascript
// Dangerous - allows HTML injection
function UserProfile({ userData }) {
    return <div dangerouslySetInnerHTML={{__html: userData.bio}} />;
}
```

**Secure Code:**
```javascript
// Safe - React escapes by default
function UserProfile({ userData }) {
    return <div>{userData.bio}</div>;
}
// Or use DOMPurify for rich text
```

---

# Pattern 6: Weak Cryptography (1/2)

**Vulnerable Code:**
```python
import hashlib

# BAD: MD5 is broken
password_hash = hashlib.md5(password.encode()).hexdigest()

# BAD: No salt
password_hash = hashlib.sha256(password.encode()).hexdigest()
```

---

# Pattern 6: Weak Cryptography (2/2)

**Secure Code:**
```python
import bcrypt

# GOOD: Use bcrypt with automatic salting
password_hash = bcrypt.hashpw(
    password.encode('utf-8'), 
    bcrypt.gensalt()
)

# Verification
if bcrypt.checkpw(password.encode('utf-8'), stored_hash):
    print("Password matches")
```

**Best Practices:** Use bcrypt, Argon2, or scrypt for passwords

---

<!-- _class: lead -->
# Part 3: Secure Code Review
## OWASP Code Review Guide

---

# What is Secure Code Review?

**Definition:** Manual examination of source code to identify security vulnerabilities

**Goals:**
- Find vulnerabilities automated tools miss
- Validate security requirements
- Identify business logic flaws
- Ensure secure coding practices
- Educate developers

**Reference:** OWASP Code Review Guide v2.0

---

# Code Review vs Security Code Review (1/3)

**Regular Code Review:**
- Functionality correctness
- Code style and readability
- Performance optimization
- Best practices

---

# Code Review vs Security Code Review (2/3)

**Security Code Review:**
- All of the above PLUS:
- Security vulnerabilities
- Attack surface analysis
- Input validation

---

# Code Review vs Security Code Review (3/3)

**More Security Focus:**
- Authentication/authorization logic

**Key Difference:**
- Regular review asks: "Does it work?"
- Security review asks: "Can it be abused?"

**Mindset:** Think like an attacker while reviewing code

---

# OWASP Code Review Checklist (1/3)

**1. Input Validation**
- All inputs validated (whitelist preferred)
- Validation on server-side
- Reject invalid input, don't try to fix
- Proper encoding/escaping

**2. Authentication & Session Management**
- Strong password requirements
- Secure session ID generation
- Proper session timeout
- Session regeneration after login

---

# OWASP Code Review Checklist (2/3)

**3. Access Control**
- Authorization checks on all operations
- Deny by default
- Least privilege principle
- Direct object reference checks

**4. Cryptography**
- Strong algorithms (AES-256, SHA-256+)
- Secure key management
- No hard-coded keys
- Proper random number generation

---

# OWASP Code Review Checklist (3/3)

**5. Error Handling**
- Generic error messages to users
- Detailed logging server-side
- No stack traces in responses
- Fail securely (deny by default)

**6. Logging**
- Security events logged
- Sufficient context (who, what, when)
- No sensitive data in logs
- Centralized log management

---

# Manual Code Review Process (1/3)

**Step 1: Understand the Application**
- Architecture and data flow
- Security requirements
- Attack surface
- Trust boundaries

**Step 2: Identify Entry Points**
- User inputs (forms, APIs, files)
- External integrations
- Configuration files

---

# Manual Code Review Process (2/3)

**Step 3: Trace Data Flow**
- Follow untrusted data through code
- Check validation at boundaries
- Look for dangerous sinks (SQL, exec, eval)
- Verify output encoding

**Step 4: Review Critical Functions**
- Authentication logic
- Authorization checks
- Cryptographic operations
- Session management

---

# Manual Code Review Process (3/3)

**Step 5: Look for Patterns**
- Common vulnerability patterns
- Security anti-patterns
- Missing security controls
- Code smells

**Step 6: Document Findings**
- Clear description
- Code location
- Risk severity
- Remediation guidance

---

# Code Review Best Practices (1/2)

**DO:**
- Review small chunks (200-400 lines)
- Use automated tools first
- Focus on high-risk areas
- Check both positive and negative cases
- Document security decisions
- Use checklists

---

# Code Review Best Practices (2/2)

**DON'T:**
- Review for hours without breaks
- Skip understanding context
- Ignore "obvious" or "simple" code
- Assume frameworks handle security
- Rush through reviews
- Only look for known patterns

**Remember:** Attackers only need one vulnerability!

---

<!-- _class: lead -->
# Part 4: Tools for Code Analysis
## Semgrep, OWASP ZAP, and More

---

# SAST Tools Overview

**Open Source:**
- **Semgrep** - Lightweight, fast, multi-language
- **Bandit** - Python security linter
- **Brakeman** - Ruby on Rails security scanner
- **FindSecBugs** - Java security audit

**Commercial:**
- SonarQube (has free tier)
- Checkmarx
- Veracode
- Fortify

---

# Semgrep: Modern SAST Tool (1/3)

**What is Semgrep?**
- Open-source static analysis tool
- Pattern-based code scanning
- Supports 30+ languages
- Fast and lightweight

---

# Semgrep: Modern SAST Tool (2/3)

**More Features:**
- Easy custom rule creation

**Use Cases:**
- Finding security vulnerabilities
- Enforcing coding standards
- Custom security policies

---

# Semgrep: Modern SAST Tool (3/4)

**More Use Cases:**
- CI/CD integration

**Example Semgrep Rule:**
```yaml
rules:
  - id: hardcoded-password
    pattern: PASSWORD = "..."
    message: Hard-coded password detected
    severity: ERROR
    languages: [python]
```

---

# Semgrep: Modern SAST Tool (4/4)

**Rule Packs Available:**
- OWASP Top 10
- Security best practices
- Language-specific rules
- Framework rules (Django, Flask, Express)

---

# Running Semgrep

**Installation:**
```bash
pip install semgrep
```

**Basic Usage:**
```bash
# Scan with default rules
semgrep --config=auto .

# Scan with OWASP Top 10 rules
semgrep --config=p/owasp-top-ten .

# Scan specific language
semgrep --config=p/python .
```

**Output:** File paths, line numbers, vulnerability descriptions

---

# DAST Tools Overview

**Open Source:**
- **OWASP ZAP** - Full-featured web app scanner
- **Nikto** - Web server scanner
- **w3af** - Web application attack framework

**Commercial:**
- Burp Suite Professional
- Acunetix
- Netsparker
- AppScan

**For this course:** OWASP ZAP (covered in lab)

---

# OWASP ZAP Overview (1/3)

**What is OWASP ZAP?**
- Zed Attack Proxy
- Free, open-source DAST tool
- Active and passive scanning
- Integrated proxy

---

# OWASP ZAP Overview (2/3)

**More Features:**
- Extensive API for automation

**Key Features:**
- Automated scanners
- Manual testing tools
- API testing support

---

# OWASP ZAP Overview (3/4)

**More Key Features:**
- CI/CD integration
- Extensible with add-ons

**Scan Types:**
- Passive Scanning
- Active Scanning

---

# OWASP ZAP Overview (4/4)

**Passive Scanning:**
- Analyzes traffic without attacks
- Safe for production
- Finds obvious issues

**Active Scanning:**
- Sends malicious payloads
- Potentially dangerous
- Use on test environments only

---

# ZAP Scanning Modes (1/2)

**1. Automated Scan**
```bash
zap-baseline.py -t https://target.com
```
- Quick baseline scan
- Passive checks only
- Good for CI/CD

---

# ZAP Scanning Modes (2/2)

**2. Full Scan**
```bash
zap-full-scan.py -t https://target.com
```
- Active + passive scanning
- Comprehensive testing
- Longer duration

---

# Other Essential Tools (1/2)

**Dependency Scanning:**
- **npm audit** - Node.js dependencies
- **pip-audit** - Python dependencies
- **OWASP Dependency-Check** - Multi-language
- **Snyk** - Comprehensive SCA tool

**Secrets Detection:**
- **git-secrets** - Prevents committing secrets
- **TruffleHog** - Finds secrets in git history
- **detect-secrets** - Pre-commit hook

---

# Other Essential Tools (2/2)

**Container Security:**
- **Trivy** - Container vulnerability scanner
- **Clair** - Static analysis for containers
- **Docker Scout** - Docker image analysis

**Cloud Security:**
- **Prowler** - AWS security assessment
- **ScoutSuite** - Multi-cloud security audit
- **Checkov** - Infrastructure as Code scanning

---

<!-- _class: lead -->
# Part 5: CI/CD Pipeline Integration
## Automating Security Testing

---

# What is CI/CD Security Integration?

**Continuous Integration/Continuous Deployment (CI/CD):**
- Automated build and deployment pipeline
- Code → Build → Test → Deploy

**Security Integration (DevSecOps):**
- Automated security testing at each stage
- Security gates in pipeline
- Fast feedback to developers
- Shift-left security approach

**Goal:** Find and fix vulnerabilities before production

---

# Security in CI/CD Stages (1/3)

**1. Code Commit Stage**
- Pre-commit hooks (secrets detection)
- SAST scanning
- Linting and code style checks
- Fast feedback (< 5 minutes)

**2. Build Stage**
- Dependency scanning (SCA)
- Container image scanning
- Build artifact signing

---

# Security in CI/CD Stages (2/3)

**3. Test Stage**
- DAST scanning (deployed to test env)
- IAST during functional testing
- API security testing
- Infrastructure scanning

**4. Deploy Stage**
- Security configuration checks
- Runtime security policies
- Compliance validation

---

# Security in CI/CD Stages (3/3)

**5. Production Monitoring**
- Runtime application protection (RASP)
- Log monitoring and SIEM
- Vulnerability management
- Incident response

**Key Principle:** Security at every stage, not just at the end!

---

# Pipeline Security Best Practices (1/2)

**1. Security Gates**
- Define severity thresholds
- Block builds on critical/high findings
- Allow warnings to pass with review

**Example Policy:**
- Critical vulnerabilities: Block build
- High vulnerabilities: Require approval
- Medium/Low: Create tickets, don't block

---

# Pipeline Security Best Practices (2/2)

**2. Performance Optimization**
- Run fast checks early (SAST)
- Run slow checks later (DAST)
- Parallel execution where possible
- Incremental scanning (only changed code)

**3. Developer Experience**
- Clear, actionable feedback
- Link to remediation guidance
- Integrate with IDE
- Minimize false positives

---

# Example GitHub Actions Workflow (1/2)

```yaml
name: Security Scan

on: [push, pull_request]

jobs:
  sast:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Run Semgrep
        uses: returntocorp/semgrep-action@v1
        with:
          config: p/owasp-top-ten
```

---

# Example GitHub Actions Workflow (2/2)

```yaml
  dependency-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Run npm audit
        run: npm audit --audit-level=high
        
  dast:
    runs-on: ubuntu-latest
    steps:
      - name: ZAP Baseline Scan
        uses: zaproxy/action-baseline@v0.7.0
        with:
          target: 'https://staging.example.com'
```

---

# Handling Security Findings (1/2)

**Triage Process:**
1. **Automated Classification**
   - Severity (Critical/High/Medium/Low)
   - Confidence (Certain/Likely/Possible)
   - Exploitability

2. **Manual Review**
   - Validate findings (remove false positives)
   - Assess business impact
   - Prioritize remediation

---

# Handling Security Findings (2/2)

**Remediation Workflow:**
1. **Critical/High:** Immediate fix, block release
2. **Medium:** Fix in current sprint
3. **Low:** Create backlog ticket

**Documentation:**
- Security finding tracker
- Remediation status
- Acceptance of risk (if applicable)
- Lessons learned

---

# Measuring Security Program Success

**Key Metrics:**
- Mean Time to Detect (MTTD)
- Mean Time to Remediate (MTTR)
- False positive rate
- Code coverage
- Vulnerability density (vulns per 1000 LOC)
- Security findings by severity

**Goal:** Continuous improvement, not perfection

---

<!-- _class: lead -->
# Key Takeaways

---

# Summary (1/2)

**1. SAST vs DAST vs IAST**
- SAST: Early, fast, high false positives
- DAST: Runtime, realistic, slower
- IAST: Best of both, complex setup
- Use multiple approaches together

**2. Common Vulnerability Patterns**
- SQL injection: Use parameterized queries
- Hard-coded secrets: Use environment variables
- XSS: Proper output encoding
- Weak crypto: Modern algorithms + proper implementation

---

# Summary (2/2)

**3. Secure Code Review**
- Manual review finds business logic flaws
- Use OWASP checklist
- Think like an attacker
- Document and share findings

**4. CI/CD Integration**
- Automate security testing
- Fast feedback loops
- Security gates with thresholds
- Continuous monitoring

**Remember:** Security is everyone's responsibility!

---

# Connection to Other Chapters

**Building on Previous Knowledge:**
- Chapters 2-4: What vulnerabilities exist (OWASP Top 10)
- **Chapter 5:** How to find them (tools and techniques)

**Looking Ahead:**
- Chapter 6: Advanced web attacks (XSS, CSRF, XXE)
- Chapter 7: Business logic vulnerabilities
- Chapter 10: Complete security assessment methodology

**Complete Picture:** Theory → Detection → Prevention → Assessment

---



# Resources for Further Learning (1/2)

**OWASP Resources:**
- OWASP Code Review Guide v2.0
- OWASP ASVS (Application Security Verification Standard)
- OWASP Testing Guide

**Tools Documentation:**
- Semgrep: https://semgrep.dev/docs/
- OWASP ZAP: https://www.zaproxy.org/docs/
- Semgrep Registry: https://semgrep.dev/r

---

# Resources for Further Learning (2/2)

**Books:**
- "The Art of Software Security Assessment"
- "Secure Coding in C and C++"
- "Web Application Security" by Andrew Hoffman

**Practice Platforms:**
- Secure Code Warrior
- OWASP WebGoat
- HackTheBox

---

<!-- _class: lead -->
# Questions?

**Next Class:** Chapter 5 Practical Lab
Hands-on with Semgrep and OWASP ZAP

---

<!-- _class: lead -->
# Thank You!

**Stay Secure, Code Smart**
