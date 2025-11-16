---
marp: true
theme: default
paginate: true
header: 'Chapter 1: Introduction to Application Security Fundamentals'
footer: 'Web and Mobile Security Course'
---

<!-- _class: lead -->
# Chapter 1
## Introduction to Application Security Fundamentals

**Web and Mobile Security Course**

---

# Learning Objectives

By the end of this lecture, you will be able to:

- Explain the CIA Triad and its importance in security
- Understand OWASP's mission and key resources
- Compare web and mobile security landscapes
- Identify common attack vectors and threats
- Recognize the importance of security in SDLC

---

<!-- _class: lead -->
# Part 1: The CIA Triad
## Foundation of Information Security

---

# The CIA Triad

The three core principles of information security:

![width:800px](https://upload.wikimedia.org/wikipedia/commons/thumb/6/65/CIA_Triad.svg/800px-CIA_Triad.svg.png)

**Confidentiality • Integrity • Availability**

<!-- Note: The CIA Triad forms the foundation of all security decisions and controls -->

---

# Confidentiality

**Definition:** Ensuring information is accessible only to authorized individuals

**Real-world examples:**
- Encryption of data in transit (HTTPS)
- Access controls and authentication
- Data classification policies
- Password protection

**Threats:** Data breaches, unauthorized access, shoulder surfing, phishing

---

# Integrity

**Definition:** Ensuring information remains accurate, complete, and unmodified by unauthorized parties

**Real-world examples:**
- Digital signatures
- Checksums and hash functions
- Version control systems
- Input validation

**Threats:** Man-in-the-middle attacks, SQL injection, data tampering

---

# Availability

**Definition:** Ensuring information and systems are accessible when needed by authorized users

**Real-world examples:**
- Redundant systems and backups
- Load balancing
- Disaster recovery plans
- DDoS protection

**Threats:** Denial of Service (DoS/DDoS), ransomware, infrastructure failures

---

# CIA Triad in Practice

| Principle | Banking App Example |
|-----------|-------------------|
| **Confidentiality** | Account details encrypted, secure login |
| **Integrity** | Transaction records cannot be altered |
| **Availability** | App accessible 24/7, even during high traffic |

**Key Insight:** All three principles must work together for effective security!

---

<!-- _class: lead -->
# Part 2: OWASP Overview
## The Global Standard for Application Security

---

# What is OWASP?

**Open Worldwide Application Security Project**

- **Founded:** December 2001
- **Type:** Non-profit foundation
- **Mission:** Improve the security of software
- **Vision:** No more insecure software

**Core Values:**
- Open: Radically transparent
- Innovative: Experimental solutions encouraged
- Global: Anyone can participate
- Integrity: Respectful, truthful, vendor-neutral

---

# OWASP Key Resources (1/2)

**1. OWASP Top 10** (Covered in Chapters 2-4)
   - The 10 most critical web application security risks
   - Updated regularly (latest: 2025 RC1)

**2. OWASP Mobile Top 10** (Covered in Chapter 9)
   - Critical mobile application security risks

---

# OWASP Key Resources (2/2)

**3. OWASP API Security Top 10** (Covered in Chapter 8)
   - API-specific vulnerabilities

---

# More OWASP Resources (1/2)

**4. OWASP Testing Guides**
   - Web Security Testing Guide (WSTG)
   - Mobile Security Testing Guide (MASTG)

**5. OWASP Projects**
   - ZAP (Zed Attack Proxy) - Security testing tool
   - Dependency-Check - Software composition analysis
   - ASVS (Application Security Verification Standard)

---

# More OWASP Resources (2/2)

**6. Community**
   - 250+ local chapters worldwide
   - Free conferences and training

---

# Why OWASP Matters

**Industry Standard:**
- Referenced in compliance frameworks (PCI-DSS, HIPAA)
- Used by security professionals globally
- Trusted by governments and enterprises

**Free and Open:**
- All resources freely available
- Community-driven development
- Vendor-neutral guidance

**Note:** Throughout this course, we'll heavily reference OWASP standards and tools

---

<!-- _class: lead -->
# Part 3: Security Landscape
## Web vs Mobile Applications

---

# Web Application Security Landscape (1/2)

**Attack Surface:**
- Server-side vulnerabilities (injection, access control)
- Client-side vulnerabilities (XSS, CSRF)
- API endpoints and integrations
- Third-party dependencies

---

# Web Application Security Landscape (2/2)

**Common Technologies:**
- Frontend: HTML, CSS, JavaScript, React, Angular, Vue
- Backend: Node.js, Python, Java, PHP, .NET
- Databases: SQL, NoSQL, Redis

---

# Web Security Challenges (1/2)

**1. Complex Tech Stacks**
   - Multiple frameworks and libraries
   - Integration points create attack vectors

**2. API-First Architecture**
   - RESTful APIs, GraphQL, WebSockets
   - Microservices increase complexity

---

# Web Security Challenges (2/2)

**3. Supply Chain Risks**
   - NPM packages, dependencies
   - Third-party scripts (analytics, ads)

**4. Browser Security Evolution**
   - Same-Origin Policy, CSP, CORS
   - New APIs introduce new risks

---

# Mobile Application Security Landscape (1/2)

**Attack Surface:**
- Insecure data storage on device
- Weak cryptography implementations
- Insecure communication channels
- Platform-specific vulnerabilities
- Physical device access

---

# Mobile Application Security Landscape (2/2)

**Platforms:**
- iOS (Swift, Objective-C)
- Android (Java, Kotlin)
- Cross-platform (React Native, Flutter)

---

# Mobile Security Challenges (1/2)

**1. Device Constraints**
   - Limited processing power affects crypto
   - Battery life impacts security features

**2. Diverse Ecosystem**
   - Multiple OS versions in use
   - Fragmented security updates (especially Android)

---

# Mobile Security Challenges (2/2)

**3. User Behavior**
   - Rooted/jailbroken devices
   - Public Wi-Fi usage
   - Lost or stolen devices

**4. App Store Security**
   - Malicious apps bypass reviews
   - Permissions abuse

---

# Web vs Mobile: Key Differences

| Aspect | Web | Mobile |
|--------|-----|--------|
| **Environment** | Browser-controlled | OS-controlled |
| **Data Storage** | Server-side primarily | Local device storage |
| **Updates** | Instant deployment | App store approval |
| **User Control** | Limited device access | Extensive permissions |
| **Attack Vector** | Network-focused | Device + Network |

**Important:** Many modern apps are hybrid, combining web and mobile risks!

---

<!-- _class: lead -->
# Part 4: Common Attack Vectors
## Understanding the Threat Landscape

---

# Attack Vector Categories (1/2)

**1. Network-Based Attacks**
- Man-in-the-Middle (MitM)
- Packet sniffing
- DNS spoofing

**2. Application-Level Attacks**
- Injection flaws (SQL, XSS, Command)
- Broken authentication
- Access control issues

---

# Attack Vector Categories (2/2)

**3. Social Engineering**
- Phishing
- Pretexting
- Baiting

---

# Top Attack Vectors (2025) - Part 1

**Based on OWASP Top 10:2025 and industry trends:**

1. **Broken Access Control** (A01:2025)
   - IDOR, privilege escalation, SSRF

2. **Injection Flaws** (A05:2025)
   - SQL, NoSQL, Command, LDAP injection

---

# Top Attack Vectors (2025) - Part 2

3. **Security Misconfiguration** (A02:2025)
   - Default credentials, verbose errors, unnecessary features

4. **Authentication Failures** (A07:2025)
   - Weak passwords, session hijacking, credential stuffing

---

# Emerging Threats (1/2)

**1. Supply Chain Attacks**
- Compromised dependencies (SolarWinds, Log4Shell)
- Malicious packages in repositories

**2. API Abuse**
- Mass assignment
- Excessive data exposure
- Rate limiting bypass

---

# Emerging Threats (2/2)

**3. Business Logic Flaws**
- Payment manipulation
- Workflow bypass
- Race conditions

**Note:** We'll cover these in detail in Chapters 2-8

---

# Attack Kill Chain (1/2)

**Understanding how attacks unfold:**

1. **Reconnaissance** - Gathering information
2. **Weaponization** - Creating exploit
3. **Delivery** - Sending exploit to target
4. **Exploitation** - Executing code on target

---

# Attack Kill Chain (2/2)

**Understanding how attacks unfold (continued):**

5. **Installation** - Installing backdoor
6. **Command & Control** - Remote control established
7. **Actions on Objectives** - Achieving attacker's goal

**Defense Strategy:** Break the chain at any stage!

---

<!-- _class: lead -->
# Part 5: Security in SDLC
## Shifting Left on Security

---

# What is Secure SDLC?

**Software Development Lifecycle (SDLC)** with security integrated at every phase

**Traditional Approach (Broken):**
- Security tested at the end
- Expensive to fix vulnerabilities
- Delayed releases

**Secure SDLC (Modern):**
- Security from day one
- Cheaper to fix early
- Continuous security validation

---

# Security Activities Across SDLC (1/3)

**1. Planning & Requirements**
- Security requirements gathering
- Threat modeling
- Risk assessment

---

# Security Activities Across SDLC (2/3)

**2. Design**
- Secure architecture design
- Attack surface analysis
- Security controls selection

---

# Security Activities Across SDLC (3/3)

**3. Implementation**
- Secure coding practices
- Code reviews
- Static analysis (SAST)

---

# Security Activities Across SDLC (4/6)

**4. Testing**
- Dynamic analysis (DAST)
- Penetration testing
- Security scanning

---

# Security Activities Across SDLC (5/6)

**5. Deployment**
- Secure configuration
- Hardening guidelines
- Access controls

---

# Security Activities Across SDLC (6/6)

**6. Maintenance**
- Patch management
- Monitoring and logging
- Incident response

---

# DevSecOps: Security as Code (1/2)

**DevSecOps** = Development + Security + Operations

**Key Principles:**
- Automate security testing in CI/CD
- Security gates in deployment pipeline
- Infrastructure as Code (IaC) security
- Continuous monitoring

---

# DevSecOps: Security as Code (2/2)

**Tools:** (We'll explore in Chapter 5)
- SAST: SonarQube, Semgrep
- DAST: OWASP ZAP
- Dependency scanning: npm audit, OWASP Dependency-Check

---

# Shift-Left Benefits (1/2)

**Earlier Detection:**
- Find bugs in development, not production
- Reduce security debt

**Cost Savings:**
- 100x cheaper to fix in development vs production
- Fewer emergency patches

---

# Shift-Left Benefits (2/2)

**Cultural Change:**
- Developers become security-aware
- Security is everyone's responsibility

**Quote:** *"Security is not a feature, it's a foundation"*

---

# Security Champions Program (1/2)

**What is it?**
A network of developers who advocate for security within their teams

**Benefits:**
- Bridge gap between security and development teams
- Faster security issue resolution
- Improved security culture

---

# Security Champions Program (2/2)

**Responsibilities:**
- Conduct code reviews with security focus
- Evangelize secure coding practices
- Stay updated on latest threats

---

<!-- _class: lead -->
# Key Takeaways

---

# Summary

1. **CIA Triad** forms the foundation: Confidentiality, Integrity, Availability

2. **OWASP** is the global standard for application security - we'll use their resources throughout this course

3. **Web and Mobile** have different but overlapping security challenges

4. **Attack Vectors** are diverse: network, application, and social engineering

5. **Secure SDLC** is essential - security must be integrated from the start

---

# Looking Ahead

**Next Chapters:**
- **Chapter 2:** OWASP Top 10 Part 1 (A01-A03)
- **Chapter 3:** OWASP Top 10 Part 2 (A04-A06)
- **Chapter 4:** OWASP Top 10 Part 3 (A07-A10)

**Next Lab:**
- Setting up DVWA
- Configuring Burp Suite
- Analyzing HTTP traffic
- Browser reconnaissance techniques

---

# Resources for Further Learning

**OWASP Resources:**
- https://owasp.org/www-project-top-ten/
- https://cheatsheetseries.owasp.org/

**Security News:**
- https://krebsonsecurity.com/
- https://thehackernews.com/

**Practice Platforms:**
- DVWA (Damn Vulnerable Web Application)
- WebGoat
- OWASP Juice Shop

---

<!-- _class: lead -->
# Questions?

**Next Class:** Chapter 1 Practical Lab
Setting up your security testing environment

---

<!-- _class: lead -->
# Thank You!
