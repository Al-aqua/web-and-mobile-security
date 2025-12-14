# Lab 5: Static and Dynamic Code Analysis

**Duration:** 2 hours

## Learning Objectives

- Perform static analysis using Semgrep on OWASP Juice Shop source code
- Run dynamic analysis with OWASP ZAP against running Juice Shop application
- Conduct manual code review to identify vulnerabilities
- Compare SAST vs DAST findings and understand their complementary nature
- Document security assessment results with remediation recommendations

---

## Part 1: Setting Up OWASP Juice Shop Source Code

For static analysis, we need the Juice Shop source code rather than just running the container.

**Clone Juice Shop Repository:**

```bash
git clone https://github.com/juice-shop/juice-shop.git
cd juice-shop
```

**Install Dependencies:**

```bash
npm install
```

**Start Juice Shop for Dynamic Analysis:**

```bash
npm start
```

Access the application at `http://localhost:3000`

---

## Part 2: SAST with Semgrep - Static Code Analysis

### Exercise 1: Install Semgrep

**Install Semgrep:**

```bash
pip install semgrep
```

**Verify Installation:**

```bash
semgrep --version
```

### Exercise 2: Basic Semgrep Scanning

**Scan with OWASP Top 10 Rules:**

```bash
cd juice-shop
semgrep --config p/owasp-top-ten .
```

**Alternative: Use Auto Configuration:**

```bash
semgrep --config auto .
```

**Scan with JavaScript/TypeScript Rules:**

```bash
semgrep --config p/javascript .
```

**Alternative: Scan with Security Audit Rules:**

```bash
semgrep --config p/security-audit .
```

**Expected Output:**
- File paths with vulnerabilities
- Line numbers
- Severity levels (ERROR, WARNING, INFO)
- Vulnerability descriptions

### Exercise 3: Targeted SQL Injection Analysis

**Scan for SQL Injection Patterns:**

```bash
semgrep --config p/sql-injection .
```

**Alternative: Scan for Express.js Security Issues:**

```bash
semgrep --config p/expressjs .
```

**Analyze Findings:**
Look for patterns like:
- String concatenation in database queries
- User input directly in SQL statements
- Missing parameterization



---

## Part 3: DAST with OWASP ZAP - Dynamic Analysis

**Reference:** For complete ZAP documentation, see the official getting started guide: https://www.zaproxy.org/getting-started/

### Exercise 5: Install OWASP ZAP Desktop

**Download OWASP ZAP:**

Visit https://www.zaproxy.org/download/ and download the installer for your platform (Windows, macOS, or Linux).

**Install and Launch:**

1. Run the installer and complete the installation
2. Launch ZAP Desktop application
3. Select "No, I do not want to persist this session" (for this lab)
4. Ensure Juice Shop is running on `http://localhost:3000`

### Exercise 6: Running an Automated Scan

ZAP's Automated Scan feature combines spidering and active scanning in a single workflow, making it ideal for quick security assessments.

**Step 1: Start Automated Scan**

1. In ZAP Desktop, look for the **"Automated Scan"** tab in the top section
2. Enter the target URL: `http://localhost:3000`
3. Select **"Use traditional spider"** (recommended for JavaScript-heavy apps like Juice Shop)
4. Check the **"Use ajax spider"** option as well for better coverage
5. Click **"Attack"** button to start the automated scan

**Step 2: Monitor Scan Progress**

The automated scan will:
- **Spider Phase:** Discover all pages, links, and endpoints (watch the "Spider" tab)
- **Passive Scan:** Analyze traffic for issues without attacking (runs automatically)
- **Active Scan Phase:** Send attack payloads to test for vulnerabilities (watch the "Active Scan" tab)

**Step 3: Review Alerts**

1. Click on the **"Alerts"** tab at the bottom of ZAP
2. Alerts are organized by risk level:
   - **High (Red):** Critical vulnerabilities requiring immediate attention
   - **Medium (Orange):** Significant security issues
   - **Low (Yellow):** Minor security concerns
   - **Informational (Blue):** Security notes and observations

**Step 4: Analyze Specific Findings**

Click on individual alerts to view:
- **Description:** What the vulnerability is
- **URL:** Where it was found
- **Risk:** Severity level
- **CWE ID:** Common Weakness Enumeration identifier
- **Solution:** Recommended remediation steps
- **Request/Response:** Actual HTTP traffic showing the vulnerability

### Exercise 7: Generate Scan Report

**Export Results:**

1. Go to **Report** menu → **Generate HTML Report**
2. Choose a location to save the report
3. Open the HTML file in a browser to view formatted results

**Report Contents:**
- Executive summary of findings
- Detailed vulnerability descriptions
- Risk ratings and CVSS scores
- Remediation recommendations
- Evidence (requests/responses)

### Exercise 8: Manual API Testing with ZAP

**Test Specific API Endpoints:**

1. In the "Sites" tree on the left, expand `http://localhost:3000`
2. Navigate to REST API endpoints:
   - `/rest/user/login`
   - `/api/Products`
   - `/rest/basket/`

3. Right-click on an endpoint → **"Attack"** → **"Active Scan"** to focus testing

4. Use the **"Request Editor"** tab to manually modify and resend requests:
   - Test authentication bypass
   - Try SQL injection payloads
   - Test for authorization issues

**Manual Testing Tips:**
- Test with different user roles
- Modify JSON payloads to include injection attempts
- Check for IDOR (Insecure Direct Object References) by changing IDs

---

## Part 4: Manual Code Review Exercise

Manual code review helps identify vulnerabilities that automated tools might miss, especially business logic flaws and context-specific security issues.

### Exercise 9: Review Authentication and Authorization

**Examine File:** `routes/login.ts`

This file handles user authentication and login functionality.

**Look For:**
- How user credentials are validated
- JWT token generation and signing
- Authentication bypass possibilities
- Weak cryptographic implementations

**Questions to Answer:**
- What algorithm is used for password hashing?
- Are there any timing attacks possible?
- How are JWT tokens generated and validated?
- Is there proper rate limiting on login attempts?

**Examine File:** `lib/insecurity.ts`

This file contains intentionally insecure security utilities (as the name suggests!).

**Look For:**
- Weak hashing functions vs strong ones
- JWT token handling in `authorize()` and `isAuthorized()`
- Session management in `authenticatedUsers` map
- CAPTCHA verification weaknesses

### Exercise 10: Review Database Queries for SQL Injection

**Examine File:** `routes/search.ts`

This file handles product search functionality - a common SQL injection target.

**Look For:**
- How user search input is processed
- String concatenation in SQL queries
- Use (or lack) of parameterized queries
- Raw SQL execution vs ORM usage

**Examine File:** `models/index.ts`

This file defines the Sequelize ORM models and database structure.

**Look For:**
- Database schema definitions
- Model associations (User, Product, Basket, etc.)
- Query methods that might be vulnerable
- Data validation at the model level

**Identify Vulnerable Patterns:**
- Direct string concatenation: `"SELECT * FROM Products WHERE name = '" + userInput + "'"`
- Raw SQL queries without parameterization
- Missing input sanitization before database operations

### Exercise 11: Review Input Validation and Sanitization

**Examine File:** `lib/insecurity.ts` (Security Features section)

**Look For:**
- `sanitizeHtml()` function implementation
- What HTML tags and attributes are allowed/blocked?
- XSS prevention mechanisms
- Input encoding/decoding functions

**Examine File:** `routes/fileUpload.ts`

File upload functionality is often vulnerable to multiple attack vectors.

**Look For:**
- File type validation (whitelist vs blacklist)
- File size restrictions
- File name sanitization
- Where uploaded files are stored
- Can arbitrary files be uploaded?

**Assess Effectiveness:**
- Are validations performed on both client and server side?
- Can validations be bypassed?
- Are there encoding issues (UTF-8, Unicode bypasses)?
- Is there any allowlist vs blocklist approach?

### Exercise 12: Review Business Logic

**Examine File:** `routes/basket.ts`

Shopping basket manipulation is a common source of business logic flaws.

**Look For:**
- Authorization checks: Can users access other users' baskets?
- Price manipulation possibilities
- Quantity validation
- The `utils.solveIf()` challenge verification logic

**Questions to Answer:**
- How does the application verify basket ownership?
- Can basket IDs be enumerated?
- Are there IDOR (Insecure Direct Object Reference) vulnerabilities?
- Can negative quantities be added?

---

## Lab Exercises Summary

Complete the following and document your findings:

**Part 2: SAST with Semgrep**
- Number of findings from OWASP Top 10 scan: ________________
- SQL injection patterns found: ________________
- Most common vulnerability type: ________________
- Files with critical issues: ________________

**Part 3: DAST with OWASP ZAP**
- Pages discovered during scan: ________________
- High-risk alerts found: ________________
- SQL injection vulnerabilities: ________________
- XSS vulnerabilities: ________________
- Security misconfiguration issues: ________________

**Part 4: Manual Code Review**
- Authentication weaknesses in `routes/login.ts`: ________________
- SQL injection points in `routes/search.ts`: ________________
- Business logic flaws in `routes/basket.ts`: ________________
- Input validation gaps identified: ________________

**Overall Analysis:**
- Total unique vulnerabilities found: ________________
- SAST-only findings: ________________
- DAST-only findings: ________________
- Confirmed by both SAST and DAST: ________________

---

## Next Lab

Chapter 6: Advanced Web Security Topics
### Cross-Site Scripting (XSS), CSRF, XXE, and SSRF