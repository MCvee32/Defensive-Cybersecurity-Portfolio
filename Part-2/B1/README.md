# Part 2 - B1: Discover 5 unique weak/vulnerable security implementations

### 1. Default Credentials
Many IoT devices like home routers and smart cameras are shipped with default passwords (e.g. admin/admin)

**Why this is weak**
Users often don't change them, leaving devices vulnerable to attackers who know or can guess the default credentials

### 2. Poor Network Segmentation
A network is not seperated into seperate zone (e.g. clinet WIFI, staff WIFI)

**Why this is weak**
If an attacker or malware gains access to one part of the network, they can move laterally and reach sensitive areas

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
