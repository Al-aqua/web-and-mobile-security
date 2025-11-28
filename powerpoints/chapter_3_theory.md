---
marp: true
theme: default
paginate: true
header: 'Chapter 3: OWASP Top 10 Web - Part 2 (A04-A06)'
footer: 'Web and Mobile Security Course'
---

<!-- _class: lead -->
# Chapter 3
## OWASP Top 10 Web - Part 2 (A04-A06)

**Web and Mobile Security Course**

---

# Learning Objectives

By the end of this lecture, you will be able to:

- Understand Cryptographic Failures and prevention strategies
- Identify various Injection attack vectors and defenses
- Recognize Insecure Design patterns and architectural flaws
- Apply secure coding practices for A04-A06 vulnerabilities
- Prepare for practical lab exercises on these topics

---

<!-- _class: lead -->
# Part 1: A04:2025 Cryptographic Failures
## Weak Encryption & Key Management Issues

---

# What is A04: Cryptographic Failures?

**Definition:** Failures related to cryptography, including weak algorithms, improper key management, and related errors.

**Impact:** Ranked #4 with 32 CWEs mapped and 1.6M+ occurrences

**Key Issues:**
- Lack of cryptography or insufficiently strong cryptography
- Leaking of cryptographic keys
- Improper key management and rotation
- Missing transport layer encryption

---

# Common Cryptographic Vulnerabilities (1/2)

**1. Deprecated Algorithms**
- MD5, SHA1, DES, RC4
- Weak encryption modes (ECB)
- Inadequate key lengths

**2. Key Management Issues**
- Hard-coded keys in source code
- Weak key generation algorithms
- Missing key rotation policies
- Keys stored in plaintext

---

# Common Cryptographic Vulnerabilities (2/2)

**3. Random Number Generation**
- Predictable seeds in PRNG
- Insufficient entropy sources
- Using non-cryptographic RNG

**4. Transport Security**
- Missing TLS/HTTPS encryption
- Weak cipher suites
- No certificate validation
- Downgrade attacks

---

# Prevention Strategies (1/2)

**Strong Algorithm Selection**
- AES-256 for encryption
- SHA-256/384 for hashing
- Argon2/scrypt for password storage
- Avoid deprecated algorithms

**Proper Key Management**
- Hardware Security Modules (HSM)
- Cloud-based key management services
- Regular key rotation
- Secure key generation

---

# Prevention Strategies (2/2)

**Transport Security**
- TLS 1.3 with forward secrecy
- HTTP Strict Transport Security (HSTS)
- Certificate pinning
- Secure cipher suite configuration

**Data Protection**
- Encrypt sensitive data at rest
- Use authenticated encryption
- Proper IV/nonce generation
- Secure random number generation

---

# Real-World Impact

**Consequences of Crypto Failures:**

- **Data Breaches:** Exposure of sensitive information
- **Compliance Violations:** GDPR, PCI-DSS, HIPAA
- **Financial Loss:** Regulatory fines, reputational damage
- **Authentication Bypass:** Session hijacking, token manipulation

> **Key Insight:** Cryptography is only as strong as its implementation

---

<!-- _class: lead -->
# Part 2: A05:2025 Injection
## SQL, Command & Code Injection Attacks

---

# What is A05: Injection?

**Definition:** System flaws allowing attackers to insert malicious code or commands into input fields, tricking the system into executing them.

**Impact:** Ranked #5 with 37 CWEs mapped and 1.4M+ occurrences

**Key Statistics:**
- Highest CVE count: 62,445 vulnerabilities
- 100% test coverage - most tested category
- Includes SQL injection, XSS, command injection

---

# Injection Attack Types (1/2)

**1. SQL Injection**
- Database manipulation via malicious SQL
- Authentication bypass, data extraction
- UNION-based, blind, error-based attacks

**2. NoSQL Injection**
- NoSQL database attacks
- MongoDB, CouchDB vulnerabilities
- Operator injection attacks

---

# Injection Attack Types (2/2)

**3. Command Injection**
- OS command execution
- Shell command injection
- System compromise

**4. Cross-Site Scripting (XSS)**
- Client-side script injection
- Stored, reflected, DOM-based XSS
- Session hijacking, data theft

---

# Injection Attack Types (3/3)

**5. Other Types**
- LDAP Injection, ORM Injection
- Expression Language Injection
- Template Injection

**6. LLM Prompt Injection**
- Manipulating AI model responses
- Bypassing safety controls
- Data extraction via prompts

---

# SQL Injection Attack Example

**Vulnerable Code:**
```sql
String query = "SELECT * FROM users WHERE id='" + userId + "'";
```

**Malicious Input:**
```sql
userId = "' OR '1'='1"
```

**Resulting Query:**
```sql
SELECT * FROM users WHERE id='' OR '1'='1'
```

**Impact:** Authentication bypass, data extraction, system compromise

---

# Prevention Strategies (1/2)

**Secure Coding Practices**
- **Parameterized Queries:** Use prepared statements
- **Input Validation:** Positive validation patterns
- **Context-Aware Escaping:** Proper encoding for each context
- **ORM Tools:** Object-relational mapping frameworks
- **Least Privilege:** Minimal database permissions

**Database Security**
- Stored procedures with parameterization
- Web application firewall (WAF)
- Database activity monitoring

---

# Prevention Strategies (2/2)

**Input Validation**
- Whitelist allowed characters
- Type and length validation
- Server-side validation only

**Output Encoding**
- HTML encoding for web output
- URL encoding for parameters
- JSON encoding for APIs

> **Best Practice:** Never trust user input - always validate and sanitize

---

<!-- _class: lead -->
# Part 3: A06:2025 Insecure Design
## Architectural Flaws & Missing Controls

---

# What is A06: Insecure Design?

**Definition:** Missing or ineffective control design leading to architectural flaws and business logic vulnerabilities.

**Impact:** Ranked #6 with 39 CWEs mapped and 729K+ occurrences

**Key Focus Areas:**
- Architectural flaws and missing controls
- Business logic vulnerabilities
- Threat modeling gaps
- Secure design pattern failures

---

# Design-Level Weaknesses (1/2)

**1. Requirements Management**
- Missing security requirements
- Inadequate risk assessment
- Lack of business risk profiling

**2. Secure Design Patterns**
- Lack of threat modeling
- Missing security controls
- No reference architectures

---

# Design-Level Weaknesses (2/2)

**3. Business Logic Flaws**
- Workflow bypass vulnerabilities
- Race conditions in transactions
- Excessive trust in client-side controls

**4. Trust Boundary Violations**
- Improper data segregation
- Missing tenant isolation
- Insecure component integration

---

# Prevention Strategies (1/2)

**Secure Design Methodology**
- **Threat Modeling:** STRIDE, PASTA methodologies
- **Secure Development Lifecycle:** Security by design
- **Design Patterns Library:** Reusable secure components
- **Security in User Stories:** Integration of security requirements

**Architecture Security**
- **Architecture Reviews:** Regular security assessments
- **Secure Reference Architectures:** Proven patterns
- **Component Security:** Trusted libraries and frameworks

---

# Prevention Strategies (2/2)

**Development Process**
- **Secure Coding Standards:** Established guidelines
- **Security Testing:** Integration in CI/CD pipeline
- **Code Reviews:** Security-focused reviews
- **Penetration Testing:** Regular security assessments

**Business Logic Security**
- **Workflow Validation:** Proper state management
- **Rate Limiting:** Abuse prevention
- **Anti-automation:** Bot protection measures

---

# Example Scenarios

**Business Logic Vulnerabilities:**

**Scenario 1:** Insecure credential recovery using security questions
- **Issue:** Questions can be guessed or researched
- **Solution:** Multi-factor authentication, token-based recovery

**Scenario 2:** Race condition in financial transactions
- **Issue:** Concurrent operations bypass validation
- **Solution:** Atomic operations, proper locking mechanisms

**Scenario 3:** Bots purchasing limited products
- **Issue:** Scalpers buying high-demand items
- **Solution:** Anti-bot design, purchase frequency limits

---

# Interconnections & Defense in Depth

**How A04-A06 Relate:**

- **A04 (Crypto)** protects data in transit/at rest
- **A05 (Injection)** prevents code execution
- **A06 (Design)** provides foundation for security

**Defense in Depth Approach:**
1. **Secure Design** (A06) - Foundation
2. **Input Validation** (A05) - First line of defense
3. **Encryption** (A04) - Data protection

---

# Key Takeaways

**Essential Points:**

1. **A04 Cryptographic Failures:** Use strong, modern crypto with proper key management
2. **A05 Injection:** Never trust user input - use parameterized queries
3. **A06 Insecure Design:** Security must be designed from the start
4. **Defense in Depth:** Multiple layers of security controls
5. **Continuous Testing:** Regular security assessments and penetration testing

---

# Questions & Discussion

**Next Steps:**

- **Lab Session:** Hands-on exploitation practice
- **Reading:** OWASP Testing Guide chapters
- **Tools:** Burp Suite, OWASP ZAP, sqlmap
- **Assessment:** Chapter 3 quiz and lab submission

---

<!-- _class: lead -->
# Thank You!

## Chapter 3 Complete
### Next: Chapter 4 - OWASP Top 10 Web - Part 3 (A07-A10)