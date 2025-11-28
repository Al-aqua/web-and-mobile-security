# Lab 3: OWASP Top 10 A04-A06 (Injection & Cryptographic Failures)

**Duration:** 2 hours

## Learning Objectives

- Exploit SQL injection vulnerabilities in OWASP Juice Shop
- Identify and analyze cryptographic failures
- Understand insecure design patterns and business logic flaws
- Practice systematic vulnerability testing with Burp Suite
- Document findings and recommend remediation strategies

---

## Part 1: Setting Up OWASP Juice Shop

OWASP Juice Shop contains verified challenges for A04-A06 vulnerabilities.

**Start Juice Shop with Docker:**

```bash
docker pull bkimminich/juice-shop
docker run --rm -p 3000:3000 bkimminich/juice-shop
```

**Access the application:**

Browse to `http://localhost:3000`

**Configure Burp Suite Proxy:**

1. Ensure Burp Suite is running from Lab 1
2. Configure browser to proxy through `127.0.0.1:8080`
3. Keep Burp Suite HTTP History open to observe traffic

---

## Part 2: A05 Injection - SQL Injection Attacks

### Exercise 1: Login Admin (⭐⭐)

**Challenge:** Log in with the administrator's user account using SQL injection

**Step 1: Analyze Login Endpoint**

1. Navigate to the login page
2. Enter any email/password combination
3. In Burp Suite HTTP History, find the POST request to `/rest/user/login`
4. Examine the JSON structure: `{"email":"...","password":"..."}`

**Step 2: Test SQL Injection**

1. Send the login request to Burp Suite Repeater
2. Try SQL injection payload in email field:
   ```json
    {"email":"admin@juice-sh.op'))--","password":"anything"}
   ```
3. Click "Send" and examine the response

**Expected Result:** Successful authentication with admin token

**What you should learn:**
- SQL injection can bypass authentication
- Comment syntax (`--`) terminates SQL queries
- JSON endpoints are vulnerable to injection

---

### Exercise 2: User Credentials (⭐⭐⭐⭐)

**Challenge:** Retrieve a list of all user credentials via SQL injection

**Step 1: Find Injection Point**

1. Browse to product search functionality
2. Search for any product (e.g., "juice")
3. In Burp Suite, find GET request to `/rest/products/search`
4. Notice parameter: `?q=juice`

**Step 2: Craft UNION SELECT Attack**

1. Send search request to Repeater
2. Test for SQL injection with single quote: `?q='))`
3. If error occurs, try UNION SELECT:
   ```
    ?q=')) UNION SELECT email,password FROM users--
   ```
4. Adjust column count if needed:
   ```
    ?q=')) UNION SELECT null,email,password,null,null FROM users--
   ```

**Expected Result:** JSON response containing user credentials

**Impact:** Complete credential database exposure

---

### Exercise 3: Database Schema (⭐⭐⭐)

**Challenge:** Exfiltrate the entire DB schema definition via SQL injection

**Step 1: Identify Database Type**

1. Use SQL injection to trigger database error
2. Analyze error message for database type (SQLite, MySQL, etc.)
3. **Hint:** Juice Shop uses SQLite

**Step 2: Extract Schema Information**

1. Use SQLite system tables:
   ```
    ?q=apple')) UNION SELECT 1,2,3,4,5,6,7,8,9 FROM sqlite_master --
   ```
2. Extract table definitions:
   ```
    ?q=apple')) UNION SELECT sql,2,3,4,5,6,7,8,9 FROM sqlite_master --
   ```

**Expected Result:** Complete database schema including table structures

**What you should learn:**
- System tables contain metadata
- Schema extraction enables targeted attacks
- Different databases have different system tables

---

## Part 3: A04 Cryptographic Issues

### Exercise 4: Weird Crypto (⭐⭐)

**Challenge:** Inform the shop about an algorithm or library it should not use

**Step 1: Analyze Source Code**

1. Open browser Developer Tools (F12)
2. Examine JavaScript files in Sources tab
3. Search for crypto-related functions:
   - Look for `crypto`, `encrypt`, `decrypt`, `hash`
   - Check for weak algorithms like `md5`, `sha1`

**Step 2: Identify Weak Crypto**

1. Search for password hashing or storage mechanisms
2. Look for insecure hashing algorithms (e.g., MD5, SHA1)
3. Check for proper use of salt and key stretching

**Step 3: Report Finding**

1. Navigate to Customer Feedback form
2. Report the specific cryptographic weakness found
3. Example: "Using MD5 for password hashing"

**Expected Result:** Challenge solved when valid crypto issue reported

---

## Part 4: A06 Insecure Design - Business Logic Analysis

### Exercise 5: Business Logic Flaw Identification

**Challenge:** Identify and document business logic vulnerabilities

**Step 1: Analyze Workflows**

1. Test the ordering workflow for logic flaws:
   - Can you order negative quantities?
   - Can you manipulate prices during checkout?
   - Are there race conditions in stock management?

**Step 2: Test Price Manipulation**

1. Add product to basket
2. In Burp Suite, intercept basket update request
3. Try modifying product price in request:
   ```json
   {"productId":1,"quantity":1,"price":0.01}
   ```
4. Check if server validates price

**Step 3: Document Findings**

1. Record any business logic vulnerabilities found
2. Note potential impact of each flaw
3. Recommend secure design patterns

---

## Lab Exercises Summary

Complete the following and document your findings:

**Exercise 1: Login Admin**
- SQL injection payload used: ________________
- Admin token obtained: Yes / No

**Exercise 2: User Credentials**
- Number of users extracted: ________________
- Password hashes obtained: Yes / No

**Exercise 3: Database Schema**
- Number of tables found: ________________
- Key tables identified: ________________

**Exercise 4: Weak Crypto**
- Crypto issue found: ________________
- Report submitted: Yes / No

**Exercise 5: Business Logic**
- Logic flaws found: ________________
- Price manipulation successful: Yes / No

---

## Next Lab

Chapter 4: OWASP Top 10 Web - Part 3 (A07-A10)
### Authentication Failures, Integrity Issues, Logging & Exception Handling