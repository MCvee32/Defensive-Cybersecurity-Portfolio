# Part 1 - A4: Discover A Vulnerable Website

### Website Discovered
http://www.perth-plumbers.com

This is an outdated webiste for a small perth plumbing business.

**Security Vulnerabilities**
- This website lacks https/tls encryption, leaving it vulnerable to "man in the middle" attacks. This places data integrity at risk, as communications can be intercepted and modified.
- This site has contact and quote request forms. User submitted data is vulnerable to "man in the middle" attacks and can include clients' names, phone numbers, and email addresses, threatening the confidentiality security principle.
