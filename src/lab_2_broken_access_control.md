# Lab 2: Broken Access Control and Security Misconfiguration

**Duration:** 2 hours

## Learning Objectives

- Identify and exploit broken access control vulnerabilities
- Test for horizontal and vertical privilege escalation
- Discover security misconfigurations through reconnaissance
- Practice systematic vulnerability testing with Burp Suite

---

## Part 1: Setting Up OWASP Juice Shop

OWASP Juice Shop is a modern intentionally insecure web application designed for security training.

**Install with Docker:**

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

## Part 2: Introduction - Finding the Score Board

**Challenge:** Find the hidden Score Board page

The Score Board tracks your progress in solving Juice Shop challenges. It's intentionally hidden to teach reconnaissance skills.

**Exercise 1: Discover the Score Board**

1. Browse the Juice Shop application normally
2. Open browser Developer Tools (F12) > Sources/Debugger tab
3. Examine the JavaScript files under `main.js` or inspect the client-side routing
4. **Hint:** Look for Angular routes - the Score Board path follows the pattern `/#/something`
5. Alternatively, try common paths: `/#/score-board`, `/#/scoreboard`, etc.

**What you should learn:**

- Hidden functionality may exist without visible links
- Client-side code often reveals API endpoints and routes
- Reconnaissance is crucial before exploitation

---

## Part 3: Broken Access Control - View Another User's Basket

**Challenge:** Access someone else's shopping basket (Horizontal Privilege Escalation)

**Exercise 2: Analyze Basket Requests**

1. Create an account and log in to Juice Shop
2. Add any product to your shopping basket
3. Navigate to the basket page
4. In Burp Suite HTTP History, find the GET request to `/rest/basket/`
5. Examine the request - notice it includes a basket ID parameter

**Questions:**

- What parameter identifies your basket?
- Is there any authorization check mentioned in the response?

**Exercise 3: Access Another Basket**

1. In Burp Suite, right-click the basket request > "Send to Repeater"
2. In Repeater tab, change the basket ID to another number (try `1`, `2`, `3`)
3. Click "Send" and examine the response
4. **Success:** You'll see another user's basket contents

**Impact:** Attacker can spy on shopping behavior, manipulate quantities, cause confusion at checkout

---

## Part 4: Broken Access Control - Admin Section Access

**Challenge:** Find and access the administration section

**Exercise 4: Discover Admin Panel**

1. Using Developer Tools > Sources, search JavaScript files for "admin"
2. Look for routes containing "administration" or similar
3. **Hint:** Try navigating to `/#/administration`
4. You'll likely see "403 Forbidden" - authorization is checked

**Exercise 5: Analyze Admin Access Check**

1. Create a regular user account if not already logged in
2. Try accessing `/#/administration` - observe the error
3. In Burp Suite, examine requests to `/rest/user/whoami` or similar
4. Look for user role information in responses

**Exercise 6: Gain Admin Access**

**Method:** Register or login with admin account

1. Find the admin email through reconnaissance (check "About Us" page, reviews, etc.)
2. Admin email format: `admin@juice-sh.op`
3. Use SQL Injection on login form to bypass authentication:
   - Email: `admin@juice-sh.op'--`
   - Password: anything
4. After successful login, access `/#/administration`

**What you should learn:**

- Access controls can be bypassed through authentication flaws
- Role-based access requires proper server-side validation
- Client-side restrictions alone are insufficient

---

## Part 5: Broken Access Control - Five Star Feedback

**Challenge:** Delete all 5-star customer feedback from the admin panel

**Exercise 7: Delete Feedback Entries**

1. Access the administration section (from Exercise 6)
2. Find the "Customer Feedback" section
3. Identify all feedback with 5-star ratings
4. Click the delete (trash) icon for each 5-star feedback

**Troubleshooting:**

- If delete doesn't work, check browser JavaScript console (F12 > Console)
- You may need to refresh the page
- Ensure you're properly authenticated as admin

**Impact:** Demonstrates how broken access control allows unauthorized data manipulation and censorship

---

## Part 6: Broken Access Control - Admin User Details

**Challenge:** Access administrative user information through API

**Exercise 8: Enumerate Users via API**

1. In Burp Suite, find requests to `/api/Users/` or `/rest/user/`
2. Send to Repeater
3. Try accessing `/api/Users/1`, `/api/Users/2`, etc.
4. Observe if you can retrieve admin user details without authorization

**Exercise 9: View All Users**

1. Try `/api/Users` without authentication
2. If blocked, try with your session token
3. Analyze response - can you see all users including admin?

**Impact:** User enumeration + privilege escalation = account takeover risk

---

## Lab Exercises Summary

Complete the following and document your findings:

**Exercise 1:** Find Score Board path
- Path discovered: ________________

**Exercise 2:** View another user's basket
- Basket IDs tested: ________________
- User data exposed: ________________

**Exercise 3:** Access admin section
- Method used: ________________
- Admin email found: ________________

**Exercise 4:** Delete 5-star feedback
- Number of entries deleted: ________________

**Exercise 5:** Enumerate users via API
- Number of users discovered: ________________
- Admin details found: Yes / No

---

## Next Lab

Chapter 3: SQL Injection and cryptographic failures in Juice Shop
