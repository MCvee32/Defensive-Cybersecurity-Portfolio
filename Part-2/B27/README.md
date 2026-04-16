# Part 2 - B27: Apply a learned concept in this unit to a real-world application/problem/environment.

**Concept Applied and Problem Addressed**  
This applies the crytopgraphic concepts of hashing, salting, symmetric encryption, and key derivation to the design of a password manager operation.

**Why This Is Important:**  
Password resuse is a significant vulnerability in personal cybersecurity. Using the same password across multiple services means that a single data breach can cause all owned accounts to beccome compromised. Applying cryptographic concepts into a password manager allows generation of unique passwords for each account and ensures they are securley stored in database through encryption.

### Application
1. Key Derivation - PBKDF2 and Salting
The user's master password is never stored directly. Instead, it used as an input to Key Derivation Function (PBKFF2). This transforms a human-memorable password into a fixed-length cryptographic key suitable for use with a symmetric cipher.

A random salt is incorporated into the derivation. This ensures that:
- Two users with identical master passwords produce entirely different derived keys.
- Pre-computed rainbow table attacks are rendered ineffective.
- The high iteration count makes brute-force attacks computationally expensive.  

**Example Code using Web Crypto API**  
```
// Key derivation using PBKDF2 (Web Crypto API)
const salt = crypto.getRandomValues(new Uint8Array(16));
const baseKey = await crypto.subtle.importKey(
  'raw', new TextEncoder().encode(masterPassword),
  'PBKDF2', false, ['deriveKey']
);
const derivedKey = await crypto.subtle.deriveKey(
  { name: 'PBKDF2', salt, iterations: 310000, hash: 'SHA-256' },
  baseKey, { name: 'AES-GCM', length: 256 }, false, ['encrypt', 'decrypt']
);
```

2. Symmetric Encryption - AES-256-GCM
Each stored password is encrypted using AES-256 in GCM. AES-256 is the industry standard for symmetric encryption a 256-bit key size means there are 2²⁵⁶ possible keys which is computationally infeasible to brute-force.

GCM mode is significant because it provides authenticated encryption. GCM produces an authentication tag that verifies the ciphertext has not been altered. This addresses both confidentiality and integrity.

A fresh random Initialisation Vector (IV) is generated for each encryption operation, so encrypting the same password twice produces different ciphertext.

**Password Encryption Storage Code**  
```
const iv = crypto.getRandomValues(new Uint8Array(12));
const ciphertext = await crypto.subtle.encrypt(
  { name: 'AES-GCM', iv },
  derivedKey,
  new TextEncoder().encode(plainPassword)
);
// Only ciphertext, salt, and iv are persisted
```

3. Unique Password Generation
When creating a new entry, the password manager generates a cryptographically random password using a Cryptographically Secure Pseudo-Random Number Generator. This ensures:
- Passwords cannot be predicted or reproduced by an attacker.
- Each site receives a unique credential. A breach at one service cannot be used to access another.
- Secure password generation policies

4. Zero-Knowledge Architecture
The master password and derived key exist only in memory during an active session and are never transmitted to a server. All encryption and decryption occurs client-side. This is known as a zero-knowledge architecture.
