# Part 2 - B1: Discover 5 unique weak/vulnerable security implementations
- I investigated the following websites to discover their strong security implementations:
  - https://oceankeysspabeauty.com.au
  - https://www.kahliaforbes.com.au
  - https://sorrentotennisclub.com.au

### 1. WordPress admin login publicly accessible at the default path (Ocean Keys Spa and Beauty)
Leaving the login at the default path (/wp-admin and /wp-login.php) exposes it to credential stuffing, brute-force attacks, and XML-RPC abuse without any additional friction. A secure WordPress installation would relocate this path, block it behind IP allowlisting, or at minimum add a CAPTCHA, which this website does not implement.

### 2. Public business email on a personal Hotmail address (Ocean Keys Spa and Beauty)
The publicly listed contact email is oceankeysspabeauty@hotmail.com. Using a free consumer email service for business communications introduces several security risks, no custom domain means no control over SPF/DKIM/DMARC email authentication records (vulnerable to email spoofing and phishing attacks), no admin-level audit logging, no ability to enforce MFA policies across the account centrally, and no data residency guarantees. A compromised Hotmail account would also directly expose all customer enquiry history.

### 3. No visible privacy policy or data handling disclosure (Kahlia Forbes Hair)
Despite the contact/enquiry form collecting names, phone numbers, and email addresses from customers, search results return no evidence of a privacy policy page on the site. Under the Australian Privacy Act 1988, any business collecting personal information is required to have a clearly accessible privacy policy. Its absence is both a legal compliance gap and a signal that data handling practices have not been formally reviewed, which matters for how securely that collected contact data is stored and managed.

### 4. Weak Authentication (Sorrento Tennis Club)
User accounts for the court booking system does not require multi-factor authentication. Therefore, attackers could potentially gain unauthorised access to members' data or bookings through brute-force attacks, credential stuffing, or phishing attacks.

### 5. Directory Listing (Sorrento Tennis Club)
Directory listing is a web server configuration flaw that causes the server to display the contents of a folder as a browsable file list when no index file is present. Rather than returning a 403 Forbidden or 404 Not Found error, the server presents a live index of all files and subdirectories within that path. On Sorrento Tennis Club's WordPress site, this misconfiguration means that attackers can browse folders within the web root, including directories containing uploaded files, plugin source code, theme files, backup archives, and potentially configuration files.

