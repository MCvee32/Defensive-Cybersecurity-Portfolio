# Part 1 - A4: Discover A Vulnerable Website

### Website Discovered
http://www.itsecgames.com/index.htm

This website was a part of listed vulnerable websites intentionally designed with security flaws for the use of legal penetration testing and cybersecurity training and practice.

**Security Vulnerabilities**
- This website has been designed with over 100 web vulnerablities
- Including all risks from the OWASP Top 10 project
- I have highlighted a few easily identifiable ones:
  - A lack of HTTPS encryption. Therfore, it is vulnerable to "man in the middle"    attacks
  - SQL Injection flaws. Input fields are not properly sanitised, allowing           attackers to manipulate the database and access sensitive information
  - Cross-Site Scripting (XSS) vulnerabilities. Allows attackers to inject           malicious scripts into the website.
