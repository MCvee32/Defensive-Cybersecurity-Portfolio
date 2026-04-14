# Part 2 - B8: Participate in a hackathon.

### Hackathon: Build with MeDo Hackathon  
**Hackathon Details:**  
https://medo.devpost.com/?_gl=1*hba89i*_gcl_au*NzEwODIwNzY1LjE3NzYwODI5Mjk.*_ga*MTQzNTY1MjgzMC4xNzc2MDgyOTMw*_ga_0YHJK3Y10M*czE3NzYxMzI0NDIkbzIkZzEkdDE3NzYxMzMyNjckajQ4JGwwJGgw  
**Requirements:**  
Build a working application using MeDo (AI software) and submit it for judging.  

### My Submission
**Project Details:**  
Password manager app used to create unique passwords and securely store encrypted passwords. Prevents password repetition vulnerabilties and stores encrypted passwords securley for user accounts.

**Deployed Application:** https://app-ay5t73enczy9.appmedo.com

## 1. Application Overview
- **Application Name:** SecureVault Password Manager
- **Description:** A browser-based password manager tool that allows users to generate strong, unique passwords, store them with encryption, organize them by category, and protect access via a PIN-based authentication mechanism.

---

## 2. Users & Use Cases
- **Target Users:** Individuals who need a secure, organized way to manage multiple passwords across different accounts.
- **Core Use Cases:**
  - Set up a PIN to protect access to the password vault
  - Generate secure, unique passwords
  - Save credentials (site/app name, username, password) under a chosen category
  - Browse and manage stored passwords by category

---

## 3. Page Structure & Feature Description

### 3.1 Overall Structure
```
SecureVault Password Manager
├── PIN Setup Page (first-time only)
├── PIN Entry Page (returning visits)
└── Vault Page
    ├── Category Sidebar / Filter
    ├── Password List Panel
    │   ├── Add New Entry
    │   └── Entry Card (view / copy / delete)
    └── Password Generator Panel
```

### 3.2 PIN Setup Page
- Displayed only on first launch when no PIN has been configured.
- User enters a numeric PIN (4–6 digits) and confirms it.
- PIN is stored locally using encryption; plain-text PIN is never persisted.
- On successful setup, user is redirected to the Vault Page.

### 3.3 PIN Entry Page
- Displayed on every subsequent visit before granting access to the vault.
- User enters their PIN to unlock the vault.
- After 5 consecutive failed attempts, access is temporarily locked for 60 seconds.
- On successful entry, user is redirected to the Vault Page.

### 3.4 Vault Page

#### 3.4.1 Category Management
- Default categories provided: Work, Personal, Finance, Social, Other.
- User can create custom categories (name only).
- User can delete a custom category; entries under it are moved to 「Other」.
- Category list displayed as a sidebar or tab filter.
- Selecting a category filters the password list to show only entries in that category.

#### 3.4.2 Password List Panel
- Displays all stored entries for the selected category.
- Each entry card shows:
  - Site / App name
  - Username / Email
  - Masked password (●●●●●●) with a toggle to reveal
  - Copy button for username and password individually
  - Delete button with confirmation prompt
- Search bar to filter entries by site/app name or username across all categories.

#### 3.4.3 Add New Entry
- Form fields:
  - Site / App name (required)
  - Username / Email (required)
  - Password (required; can be typed manually or filled via the generator)
  - Category (dropdown, required; defaults to 「Other」)
- 「Generate Password」 button opens the Password Generator Panel inline.
- On save, entry is encrypted and persisted to local storage.

#### 3.4.4 Password Generator Panel
- Accessible from the Add New Entry form and as a standalone tool within the vault.
- Options:
  - Password length: slider or numeric input (range 8–64, default 16)
  - Include uppercase letters (toggle, default on)
  - Include numbers (toggle, default on)
  - Include symbols (toggle, default on)
- Displays the generated password in a read-only field.
- 「Regenerate」 button produces a new password with the same settings.
- 「Use This Password」 button fills the password field in the Add New Entry form.
- 「Copy」 button copies the generated password to clipboard.

---

## 4. Business Rules & Logic

| Rule | Description |
|---|---|
| Encryption | All stored passwords are encrypted using AES-256 before being written to local storage. The encryption key is derived from the user's PIN via a key-derivation function (e.g., PBKDF2). |
| PIN Change | Not in scope for this release. |
| Data Persistence | All vault data is stored in the browser's local storage; no server-side storage. |
| Clipboard Auto-clear | Copied passwords are automatically cleared from the clipboard after 30 seconds. |
| Session Lock | Vault auto-locks after 10 minutes of inactivity, requiring PIN re-entry. |
| Password Strength | Generated passwords must satisfy at least 3 of the 4 character-type rules enabled by the user. |

---

## 6. Acceptance Criteria

1. First-time users are prompted to set a PIN before accessing the vault.
2. Returning users must enter the correct PIN to unlock the vault.
3. After 5 failed PIN attempts, the entry screen is locked for 60 seconds.
4. The vault auto-locks after 10 minutes of inactivity.
5. Users can add, view, copy, and delete password entries.
6. Each entry is assigned to a category and can be filtered by category.
7. The password generator produces a password matching the selected options (length, character types).
8. All stored passwords are encrypted; plain-text passwords are never written to local storage.
9. Copied passwords are cleared from the clipboard after 30 seconds.
10. Users can create and delete custom categories.

---

## 7. Out of Scope (This Release)

- PIN change or PIN recovery / reset flow
- Cloud sync or cross-device access
- Browser extension integration
- Import / export of vault data
- Two-factor authentication
- Password breach / leak detection
- Edit existing entry (entries can be deleted and re-added)…]()

## App UI
<img width="990" height="779" alt="Screenshot 2026-04-14 at 10 42 58 am" src="https://github.com/user-attachments/assets/f3d822e7-80c6-47fa-a7f2-5611e0a4366d" />

### Hackathon Project Submission
<img width="1433" height="900" alt="Screenshot 2026-04-14 at 10 45 40 am" src="https://github.com/user-attachments/assets/52aad0f2-4955-4cba-97f2-2812ce2c5dae" />
