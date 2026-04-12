# Part 2 - B2: Discover 5 unique strong security implementations
- I investigated the following websites/systems to discover their strong security implementations:
  - https://www.partymixperth.com
  - https://oceankeysspabeauty.com.au
  - https://www.xbox.com/en-AU/live
  - macOS
  
### 1. PCI-DSS compliant payment processing (Party Mix)
Party Mix's online checkout accepts Visa, Mastercard, Apple Pay, Google Pay, Shop Pay, and UnionPay, all processed entirely through Shopify Payments. Card data never touches Party Mix Perth's own servers. Shopify is a Level 1 PCI-DSS certified service provider, meaning the most sensitive part of the transaction (cardholder data entry, transmission, and storag) is handled by infrastructure that undergoes rigorous annual third-party audits. Outsourcing the hardest seccurity problem to dedicated experts is a strong security implmentation for a small retailer.

### 2. HTTPS enforced across the enitre site (Party Mix)
All pages, including product listings, account authentication, and the wholesale login, are served exclusively over HTTPS. This ensures all data in transit, including login credentials, session tokens, and browsing behaviour is encrypted and protected from interception on networks like public Wi-Fi. The TLS certificate is managed by Shopify, meaning it is automatically renewed and unlikely to expire unexpectedly

### 3. OTP-based authentication for booking system (Ocean Keys Spa and Beauty)
When a customer signs in, they enter their first name and mobile number, and receive an SMS code to authenticate. This is functionally equivalent to SMS-based MFA, and critically it means there is no stored password that can be leaked, phished, or credential-stuffed. Even if a customer's email or phone number is known, an attacker cannot log in without access to the physical device. This is a meaningfully stronger authentication design than a username/password system.

### 4. Multi-Factor Authentication (Xbox)
Implements multi-factor authentication in order to gain access to xbox account. This provides a further layer of security other than just username/password. Significantly, reducing unauthorised access.
<img width="713" height="817" alt="Screenshot 2026-04-12 at 11 02 36 am" src="https://github.com/user-attachments/assets/88eb8656-217c-4600-ad0a-900646f48b45" />

### 5. Full Disk Encryption (macOS - FileVault)
A built-in macOS security feature that encrypts the startup disk to prevent unauthorized access to data if a Mac is stolen or lost. It requires a login password to unlock the disk, protecting against unauthorized data access even if the hard drive is removed/stolen.
<img width="502" height="271" alt="Screenshot 2026-04-12 at 11 24 53 am" src="https://github.com/user-attachments/assets/df4a8d8c-fcf1-486f-9232-668643b70c39" />

