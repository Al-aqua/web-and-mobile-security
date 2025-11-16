# Lab 1: Security Testing Environment Setup

**Duration:** 2 hours

## Learning Objectives

- Install and configure DVWA and Burp Suite
- Configure browser proxy settings
- Capture and analyze HTTP traffic
- Perform basic reconnaissance

---

## Part 1: Installing DVWA with Docker

DVWA is an intentionally vulnerable web application for security training.

**Install Docker:**

```bash
# Linux (Debian/Ubuntu)
sudo apt update && sudo apt install -y docker.io
sudo systemctl start docker
sudo usermod -aG docker $USER  # Log out and back in after this
```

**Windows/Mac:** Download Docker Desktop from https://www.docker.com/products/docker-desktop

**Run DVWA:**

```bash
docker run --rm -it -p 80:80 vulnerables/web-dvwa
```

**Setup DVWA:**

1. Browse to `http://localhost`
2. Click "Create / Reset Database"
3. Login: Username `admin`, Password `password`
4. Set Security Level to **Low** (DVWA Security menu)

---

## Part 2: Installing Burp Suite

Burp Suite is an HTTP proxy for web security testing.

**Download and Install:**

1. Visit https://portswigger.net/burp/communitydownload
2. Download and install for your OS
3. Launch Burp Suite
4. Select "Temporary project" > "Use Burp defaults" > "Start Burp"

---

## Part 3: Configuring Browser Proxy

**Configure Firefox Proxy:**

1. Settings > Search "proxy" > Network Settings
2. Select "Manual proxy configuration"
3. HTTP Proxy: `127.0.0.1`, Port: `8080`
4. Check "Also use this proxy for HTTPS"

**Install Burp CA Certificate (for HTTPS):**

1. Browse to `http://burpsuite`
2. Download CA Certificate
3. Firefox Settings > Privacy & Security > Certificates > View Certificates
4. Authorities tab > Import > Select downloaded certificate
5. Check "Trust this CA to identify websites"

---

## Part 4: Capturing HTTP Traffic

**Enable Intercept:**

1. Burp Suite > Proxy > Intercept
2. Ensure "Intercept is on"
3. Browse to `http://localhost` - browser will hang
4. View intercepted request in Burp Suite
5. Click "Forward" to send request
6. Turn off intercept for normal browsing

**View HTTP History:**

1. Proxy > HTTP history shows all captured traffic
2. Click any request to see details
3. Examine Request and Response tabs

**What to Look For:**

- Cookies (session tokens)
- Server headers
- Parameters and form data
- Status codes (200, 302, 404, etc.)

---

## Part 5: Browser Developer Tools

**Open Developer Tools:** Press `F12`

**Network Tab:**

1. Refresh DVWA homepage
2. Observe all network requests
3. Examine headers, cookies, response times

**Console Tab:**

1. View JavaScript errors and logs
2. Try: `document.cookie` to see cookies

**Inspector Tab:**

1. Right-click on login form > Inspect Element
2. Find form field names and hidden inputs

---

## Lab Exercises

**Exercise 1: Capture Login**

1. Enable Burp intercept
2. Login to DVWA
3. Find parameter names for username/password in POST data

**Exercise 2: Response Headers**

1. In Burp HTTP history, view any request
2. Identify the web server (Server header)
3. Find security headers (X-Frame-Options, etc.)

**Exercise 3: Hidden Fields**

1. Navigate to DVWA's SQL Injection page
2. Use Inspector to find any hidden input fields

**Exercise 4: Cookie Analysis**

1. In Burp history, find session cookie name
2. Examine cookie value format

---

## Troubleshooting

**DVWA won't load:**
- Check Docker: `docker ps`
- Try `http://127.0.0.1`

**Burp not intercepting HTTPS:**
- Verify CA certificate is installed
- Check proxy settings: `127.0.0.1:8080`

**Browser can't connect:**
- Ensure Burp Suite is running
- Turn intercept off for normal browsing

---

## Key Takeaways

- DVWA provides a safe practice environment
- Burp Suite intercepts HTTP/HTTPS traffic
- HTTP history captures all requests
- Browser tools provide additional reconnaissance
- Understanding HTTP is fundamental to web security

## Next Lab

Chapter 2: Exploiting IDOR vulnerabilities and testing access controls
