# Part 2 - B2: Discover 5 unique strong security implementations
- I investigated the following websites to discover their strong security implementations:
  - https://www.partymixperth.com
  - https://oceankeysspabeauty.com.au
### 1. PCI-DSS compliant payment processing (Party Mix)
Party Mix's online checkout accepts Visa, Mastercard, Apple Pay, Google Pay, Shop Pay, and UnionPay, all processed entirely through Shopify Payments. Card data never touches Party Mix Perth's own servers. Shopify is a Level 1 PCI-DSS certified service provider, meaning the most sensitive part of the transaction (cardholder data entry, transmission, and storag) is handled by infrastructure that undergoes rigorous annual third-party audits. Outsourcing the hardest seccurity problem to dedicated experts is a strong security implmentation for a small retailer.

### 2. HTTPS enforced across the enitre site (Party Mix)
All pages, including product listings, account authentication, and the wholesale login, are served exclusively over HTTPS. This ensures all data in transit, including login credentials, session tokens, and browsing behaviour is encrypted and protected from interception on networks like public Wi-Fi. The TLS certificate is managed by Shopify, meaning it is automatically renewed and unlikely to expire unexpectedly

### 3. OTP-based authentication for booking system (Ocean Keys Spa and Beauty)
When a customer signs in, they enter their first name and mobile number, and receive an SMS code to authenticate. This is functionally equivalent to SMS-based MFA, and critically it means there is no stored password that can be leaked, phished, or credential-stuffed. Even if a customer's email or phone number is known, an attacker cannot log in without access to the physical device. This is a meaningfully stronger authentication design than a username/password system.

### 4. Regular Security Patching and Updates
Ensuring systems are up to date with latest security patch/updates. Can be updated automatically.
**Security Benefit:**
Provides protection against known vulnerabilities, reducing vulnerabilities to be exploited by attackers.

### 5. Hardware Security Modules (HSMs)
A physical tamper-resistant computing device used to protect cryptographic keys and accelerate cryptographic functions
**Security Benefit:**
Stores sensitive information in isolated, tamper-resistent hardware making extraction extremely difficult
