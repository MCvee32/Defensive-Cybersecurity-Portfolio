# Part 2 - B1: Discover 5 unique weak/vulnerable security implementations
- I investigated the following websites to discover their strong security implementations:
  - https://oceankeysspabeauty.com.au
### 1. WordPress admin login publicly accessible at the default path (Ocean Keys Spa and Beauty)
Leaving the login at the default path (/wp-admin and /wp-login.php) exposes it to credential stuffing, brute-force attacks, and XML-RPC abuse without any additional friction. A secure WordPress installation would relocate this path, block it behind IP allowlisting, or at minimum add a CAPTCHA, which this website does not implement.

### 2. Public business email on a personal Hotmail address (Ocean Keys Spa and Beauty)
The publicly listed contact email is oceankeysspabeauty@hotmail.com. Using a free consumer email service for business communications introduces several security risks, no custom domain means no control over SPF/DKIM/DMARC email authentication records (vulnerable to email spoofing and phishing attacks), no admin-level audit logging, no ability to enforce MFA policies across the account centrally, and no data residency guarantees. A compromised Hotmail account would also directly expose all customer enquiry history.

### 3. Unencrypted Public WIFI
A WIFI network not using encryption e.g. WPA2

**Why this is weak**
Attackers can intercept traffic, leaving WIFI users vulnerable to credential theft or data leaks.

### 4. Plaintext Storage of Sensitive Data
Storing sensitive data without encrypting it first e.g. plaintext storage of usernames and passwords, credit card info, medical info, etc.

**Why this is weak**
If the database is breached, all sensitive data is immediately exposed

### 5. Unpatched Software/Legacy Systems
Running outdated versions of a system, withour security updates

**Why this is weak**
Allows attackers to exploit known vulnerabilities (CVEs) which can be used to gain unauthorised access or execute malware.
