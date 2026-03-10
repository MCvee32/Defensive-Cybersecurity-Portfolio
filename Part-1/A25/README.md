# Part 1 - A25: Design and Implement a Privacy-Preserving Technique for an Appropriate Application

**Application**
Medical Information System used by hospitals or medical centres to store patient records such as medical history, prescriptions, and test results.

**Privacy-Preserving Technique**
Data Encryption
- Sensitive patient data is encrypted and only authorised healthcare professionals can access it.

**Implementation**
- Patients' medical records are encrypted using AES-256 beofre being stored in the database.
- Ensures that if the database is breached, attackers cannot read the data without the encryption key.
**Example Implementation**
1. Example Patient Record
<img width="1440" height="900" alt="Screenshot 2026-03-10 at 3 28 56 pm" src="https://github.com/user-attachments/assets/6ac238a6-e68e-4ce8-8049-1f314738efeb" />

2. Encrypt the Medical Record using AES-256
<img width="1440" height="900" alt="Screenshot 2026-03-10 at 3 31 39 pm" src="https://github.com/user-attachments/assets/f4f48596-1e9b-456a-a2fa-9095fa441c82" />

3. Encrypted Data/Record
<img width="1440" height="900" alt="Screenshot 2026-03-10 at 3 32 11 pm" src="https://github.com/user-attachments/assets/3b36410f-e1de-4903-9dcc-60115a96af73" />

4. Decrypt the Record
<img width="1440" height="900" alt="Screenshot 2026-03-10 at 3 34 17 pm" src="https://github.com/user-attachments/assets/ed01c90f-68ae-4487-947c-4b6b16b12e94" />

5. Decrypted Record
<img width="1440" height="900" alt="Screenshot 2026-03-10 at 3 34 44 pm" src="https://github.com/user-attachments/assets/f0a8697e-7b0b-41e2-a15e-a21c7d066950" />

**Purpose**
- Protects patient confidentiality by ensuring medical data is encrypted, even if system is compromised encrypted data cannot easily be read by attackers.
