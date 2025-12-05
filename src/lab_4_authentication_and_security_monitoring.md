# Lab 4: Authentication Failures, Integrity Issues & Security Monitoring

**Duration:** 2 hours

## Learning Objectives

- Exploit weak password mechanisms and authentication bypass
- Test session management vulnerabilities
- Identify logging and monitoring failures
- Analyze error handling and information disclosure
- Practice brute force attacks with Burp Suite Intruder

---

## Part 1: Setting Up OWASP Juice Shop

We'll continue using OWASP Juice Shop which contains verified challenges for A07-A10 vulnerabilities.

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
4. Navigate to Score Board at `/#/score-board` to track progress

---

## Part 2: A07 Authentication Failures - Weak Passwords

### Exercise 1: Password Strength (⭐⭐)

**Challenge:** Log in with the administrator's user credentials without SQL Injection

**Background:** Many applications use weak default or easily guessable passwords. The administrator account often uses common passwords that can be guessed or cracked.

**Step 1: Identify the Admin Email**

1. Browse the Juice Shop application
2. Look for the admin email in "About Us" page or customer reviews
3. Admin email: `admin@juice-sh.op`

**Step 2: Attempt Common Passwords**

Try these common administrator passwords:
- `admin123`
- `password`
- `admin`
- `12345`

**Hint:** The admin password is very simple and relates to the admin's role

**What you should learn:**
- Weak passwords are a critical vulnerability
- Default credentials should always be changed
- Password policies should enforce complexity
- Common password lists can crack weak passwords

---

### Exercise 2: Security Question Weakness (⭐⭐⭐)

**Challenge:** Reset Jim's password via the Forgot Password mechanism

**Background:** Security questions are often based on publicly available information or easy-to-guess answers.

**Step 1: Find Jim's Account**

1. Navigate to "Customer Feedback" or user reviews
2. Look for references to celebrity users
3. Jim is actually "Jim" from Star Trek (Captain Kirk)

**Step 2: Access Password Reset**

1. Go to login page and click "Forgot Password"
2. Enter Jim's email: `jim@juice-sh.op`
3. Observe the security question displayed

**Step 3: Research the Answer**

1. Jim's security question relates to his character
2. Search online for "Jim Kirk" middle name or sibling
3. **Hint:** The answer is his brother's name from Star Trek

**Step 4: Reset the Password**

1. Enter the answer to the security question
2. Set a new password
3. Log in with Jim's account using new password

**What you should learn:**
- Security questions are weak if answers are publicly available
- Celebrity accounts are vulnerable to OSINT (Open Source Intelligence)
- Modern authentication should not rely solely on knowledge-based questions
- Multi-factor authentication provides better security

---

## Part 3: A07 Authentication Failures - Brute Force with Burp Intruder

### Exercise 3: Brute Force Attack (⭐⭐⭐)

**Challenge:** Use Burp Suite Intruder to perform a brute force attack

**Background:** Brute force attacks attempt many passwords against an account. Without rate limiting, attackers can try thousands of passwords quickly.

**Step 1: Capture Login Request**

1. Attempt login to Juice Shop with any credentials
2. In Burp Suite Proxy > HTTP History, find POST to `/rest/user/login`
3. Right-click > "Send to Intruder"

**Step 2: Configure Intruder**

1. In Burp Suite, go to Intruder tab
2. Positions sub-tab:
   - Click "Clear §" to remove all position markers
   - Select the password value in JSON (e.g., `test`)
   - Click "Add §" to mark it as injection point
   - Result: `{"email":"admin@juice-sh.op","password":"§test§"}`

**Step 3: Load Payload List**

1. Go to Payloads sub-tab
2. Payload set: 1
3. Payload type: Simple list
4. Add common passwords manually:
   ```
   admin
   password
   123456
   admin123
   juice
   juiceshop
   admin123456
   password123
   letmein
   ```
5. Alternatively, load a password list from SecLists

**Step 4: Launch Attack**

1. Click "Start attack" (Community Edition will be slower)
2. Analyze responses:
   - Look for different response lengths
   - Look for HTTP status codes (200 vs 401)
   - Successful login will have longer response with authentication token

**Step 5: Identify Successful Login**

1. Sort results by "Length" column
2. Find the password that returns a different response
3. Successful authentication returns a JSON with `token` field

**What you should learn:**
- Brute force attacks are effective against weak passwords
- Rate limiting and account lockout prevent brute force
- CAPTCHA can slow down automated attacks
- Strong passwords with high entropy resist brute force

---

## Part 4: A09 Logging & Monitoring - Access Log Discovery

### Exercise 4: Exposed Metrics (⭐)

**Challenge:** Find the endpoint that serves usage data for Prometheus monitoring

**Background:** Monitoring endpoints often expose internal application metrics. While useful for operations, they can leak sensitive information about application internals.

**Step 1: Research Prometheus Endpoints**

1. Prometheus scrapes metrics from a standard endpoint
2. The default path is documented at prometheus.io
3. Common paths: `/metrics`, `/actuator/metrics`, `/prometheus`

**Step 2: Access the Metrics Endpoint**

1. Try: `http://localhost:3000/metrics`
2. Observe the output format (Prometheus format)
3. Look for interesting metrics:
   - Request counts by endpoint
   - Error rates
   - Response times
   - Active connections

**Step 3: Analyze Exposed Information**

Metrics might reveal:
- Internal API endpoints
- Usage patterns
- Performance bottlenecks
- Error rates (indicating vulnerable endpoints)

**What you should learn:**
- Monitoring endpoints should be protected
- Metrics can reveal sensitive application information
- Implement authentication on administrative endpoints
- Use network segmentation for monitoring systems

---

## Lab Exercises Summary

Complete the following and document your findings:

**Exercise 1: Password Strength**
- Admin password discovered: ________________
- Time to crack: ________________

**Exercise 2: Security Question**
- Jim's security question: ________________
- Answer found: ________________
- Information source: ________________

**Exercise 3: Brute Force**
- Number of passwords tested: ________________
- Successful password: ________________
- Response indicators: ________________

**Exercise 4: Metrics Endpoint**
- Endpoint path: ________________
- Sensitive information exposed: ________________

---

## Key Takeaways

**A07: Authentication Failures**
- Weak passwords enable easy account compromise
- Security questions based on public info are ineffective
- Rate limiting prevents brute force attacks
- Multi-factor authentication significantly improves security

**A09: Logging & Monitoring**
- Metrics endpoints can leak sensitive information
- Monitoring systems need authentication
- Logs help detect and respond to attacks
- Balance observability with security

---

## Next Lab

Chapter 5: Static and Dynamic Code Analysis (SAST/DAST)
