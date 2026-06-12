# 🔑 `passwd` Command in Linux — Complete Beginner to Enterprise Guide

> Learn how Linux manages passwords, changes credentials, locks accounts, enforces security policies, integrates with authentication systems, and protects user identities.

---

# 🎯 What is the `passwd` Command?

Imagine every house has a lock.

```text
House
  │
  ▼
Lock
  │
  ▼
Key
```

Linux users also have locks.

Their "key" is:

```text
Password
```

The command responsible for managing that key is:

```bash
passwd
```

---

# 🧠 Simple Definition

`passwd` manages user passwords and account authentication settings.

Most common use:

```bash
passwd
```

Change your own password.

Or:

```bash
sudo passwd rahul
```

Change another user's password.

---

# 🚨 Most Common Beginner Misconception

Many people think:

```text
passwd
   │
   ▼
Stores Password
```

Wrong.

Linux NEVER stores your actual password.

Linux stores:

```text
Password Hash
```

---

# Visual

```text
Password
   │
   ▼
Hash Function
   │
   ▼
Stored Hash
```

Not:

```text
mypassword123
```

Instead:

```text
$6$R3fK...
```

---

# Why Is This Important?

Suppose database leaked.

Bad:

```text
Password Stored Directly
```

Attacker sees:

```text
mypassword123
```

Immediately.

---

Good:

```text
Password Hash Stored
```

Attacker sees:

```text
$6$abc$92hd...
```

Much harder to abuse.

---

# Where Does passwd Store Information?

The command updates:

```text
/etc/shadow
```

Not:

```text
/etc/passwd
```

Visual:

```text
passwd
   │
   ▼
/etc/shadow
   │
   ▼
Authentication Data
```

---

# Basic Syntax

```bash
passwd [options] [username]
```

---

# Change Your Own Password

```bash
passwd
```

Example:

```text
Current password:
New password:
Retype new password:
```

---

# Change Another User's Password

Administrator:

```bash
sudo passwd rahul
```

Visual:

```text
Admin
  │
  ▼
passwd rahul
  │
  ▼
Password Updated
```

---

# What Happens Internally?

Suppose:

```text
New Password:
MySecurePassword123
```

Linux performs:

```text
Receive Password
      │
      ▼
Generate Salt
      │
      ▼
Create Hash
      │
      ▼
Store in /etc/shadow
```

---

# What is Salt?

One of the most important security concepts.

Without salt:

```text
password123
```

always becomes:

```text
ABC123HASH
```

Attackers can precompute huge lookup tables.

---

With salt:

```text
Salt + Password
```

Example:

```text
XYZ123 + password123
```

Result:

```text
Different Hash
```

Every user gets unique hashes.

---

# Visual

```text
Password
    │
    ▼
Add Salt
    │
    ▼
Hash
    │
    ▼
Store
```

---

# Password Verification Process

Login:

```text
User enters password
          │
          ▼
Hash entered password
          │
          ▼
Compare stored hash
          │
          ▼
Match?
```

---

Success:

```text
Login Allowed
```

Failure:

```text
Access Denied
```

---

# Password Hash Algorithms

Modern Linux may use:

```text
SHA-512
yescrypt
bcrypt
```

Older systems:

```text
MD5
SHA-256
```

may still appear.

---

# How To Check Password Status

```bash
passwd -S rahul
```

Example:

```text
rahul P 06/12/2026 0 99999 7 -1
```

---

# Understanding Output

```text
P = Password Set

L = Locked

NP = No Password
```

---

# Visual

```text
Account Status

P
│
└── Password Active

L
│
└── Locked

NP
│
└── No Password
```

---

# Locking Accounts

One of the most useful features.

Command:

```bash
sudo passwd -l rahul
```

---

What Happens?

Linux modifies:

```text
/etc/shadow
```

Before:

```text
$6$hash...
```

After:

```text
!$6$hash...
```

---

Visual

```text
Password Hash
      │
      ▼
Add !
      │
      ▼
Login Blocked
```

---

# Important Concept

Account lock ≠ account deletion

Lock:

```text
User Exists
Data Exists
Login Blocked
```

Delete:

```text
User Removed
```

---

# Unlocking Accounts

```bash
sudo passwd -u rahul
```

Removes:

```text
!
```

from password entry.

---

# Expiring Passwords

Force user to change password.

```bash
sudo passwd -e rahul
```

---

Visual

```text
Next Login
     │
     ▼
Password Change Required
```

---

# Real World Usage

Companies often do:

```text
Employee Onboarding
       │
       ▼
Temporary Password
       │
       ▼
Force Change On First Login
```

---

# Delete Password

```bash
sudo passwd -d rahul
```

---

What Happens?

Password hash removed.

---

# Dangerous Question

Can user login without password now?

Answer:

```text
Maybe
```

Depends on:

```text
SSH
PAM
Login Configuration
Distribution
```

Never use casually.

---

# Edge Case: Passwordless Accounts

Some systems intentionally use:

```text
No Password
```

for:

```text
Containers
Embedded Systems
Service Accounts
Automation Accounts
```

Must be carefully secured.

---

# Security Risk Example

Bad:

```bash
passwd -d admin
```

Potential result:

```text
Unauthorized Access
```

---

# Password Aging Concepts

Question:

```text
Why force password changes?
```

Because stolen passwords become less useful.

---

Linux supports:

```text
Minimum Age
Maximum Age
Warning Period
Expiration
Inactive Period
```

Stored in:

```text
/etc/shadow
```

---

# Visual

```text
Password Created
       │
       ▼
30 Days
       │
       ▼
Warning
       │
       ▼
Expire
```

---

# View Aging Information

```bash
sudo chage -l rahul
```

Example:

```text
Password expires
Account expires
Password inactive
```

---

# Enterprise Security Policies

Common policies:

```text
Minimum Length
Complexity Rules
History Enforcement
Expiration Rules
Lockout Rules
```

---

# Where Are These Enforced?

Through:

```text
PAM
```

---

# What is PAM?

One of the most important Linux concepts.

PAM:

```text
Pluggable Authentication Modules
```

---

Visual

```text
User Login
      │
      ▼
PAM
      │
      ▼
Authentication Rules
      │
      ▼
Allow / Deny
```

---

# Why PAM Matters

Without PAM:

```text
Every Application
Needs Authentication Logic
```

With PAM:

```text
One Central Authentication Framework
```

---

# Enterprise Authentication Flow

```text
User Login
      │
      ▼
PAM
      │
      ▼
LDAP
      │
      ▼
Active Directory
      │
      ▼
Authentication Result
```

---

# Important Edge Case

Changing password locally may not matter if user is managed by:

```text
LDAP
Active Directory
FreeIPA
SSSD
```

Because password source is external.

---

# Service Accounts

Question:

```text
Should service accounts have passwords?
```

Usually:

```text
No
```

Example:

```text
nginx
mysql
postgres
```

Often use:

```text
nologin
```

shells.

---

# Why?

Humans should not login as:

```text
mysql
```

---

# Security Risks

---

## Weak Passwords

Bad:

```text
123456
password
admin
qwerty
```

---

## Password Reuse

One breach:

```text
Email
GitHub
Linux Server
VPN
```

all compromised.

---

## Shared Accounts

Bad:

```text
admin
```

used by 10 people.

Problem:

```text
No Accountability
```

---

## Storing Passwords in Scripts

Bad:

```bash
PASSWORD=secret123
```

inside scripts.

---

# Brute Force Attacks

Attacker tries:

```text
password1
password2
password3
...
```

until success.

---

Protection:

```text
Strong Passwords
MFA
Fail2ban
Rate Limiting
```

---

# What Happens If Root Forgets Password?

Important Interview Question.

Possible recovery:

```text
GRUB Recovery
Single User Mode
Rescue Environment
```

Reset password offline.

---

# Enterprise Offboarding Workflow

Employee Leaves:

```text
Lock Account
Archive Data
Rotate Secrets
Review Access
Disable Authentication
```

Often:

```bash
passwd -l username
```

before deletion.

---

# Deep Concept

Password ≠ Identity

Identity includes:

```text
UID
Groups
SSH Keys
Tokens
Certificates
Kerberos Tickets
Passwords
```

Password is only one part.

Modern systems increasingly use:

```text
MFA
SSO
Certificates
Passkeys
```

---

# Useful Commands

Check status:

```bash
passwd -S username
```

---

Lock account:

```bash
passwd -l username
```

---

Unlock:

```bash
passwd -u username
```

---

Expire password:

```bash
passwd -e username
```

---

Delete password:

```bash
passwd -d username
```

---

Change own password:

```bash
passwd
```

---

Change another user password:

```bash
sudo passwd username
```

---

# Questions Most Tutorials Never Answer

### Does passwd store passwords?

No.

Stores hashes.

---

### Can two users have the same password?

Yes.

But salts make hashes different.

---

### Why can Linux verify passwords if it doesn't know the password?

Because hashes can be compared.

---

### Why lock instead of delete?

Preserves data and audit trail.

---

### Can deleting a password be dangerous?

Very.

May create unintended authentication paths.

---

### What happens if /etc/shadow is leaked?

Attackers can perform offline cracking attempts.

---

### Why are salts important?

Prevent rainbow-table attacks.

---

### Can root see my password?

Usually not.

Root can change it.

Root sees the hash, not the original password.

---

### Why are service accounts often passwordless?

Because humans should never authenticate as them.

---

# 🧠 Interview Deep-Dive

Difference:

```text
passwd -l
```

vs

```text
usermod -L
```

Answer:

```text
Both lock accounts,
but passwd directly manipulates password authentication.
```

---

Difference:

```text
passwd -d
```

vs

```text
passwd -l
```

Answer:

```text
-d removes password

-l locks password
```

Very different security implications.

---

# 🎯 Summary

`passwd` is not merely a password-changing command.

It is part of Linux's:

```text
Authentication System
Identity Management
Security Architecture
Access Control Framework
```

Understanding `passwd` means understanding:

```text
Hashes
Salts
PAM
Authentication
Account Security
Password Policies
Enterprise Identity Management
```

---

# 🗺️ Big Picture

```text
User
 │
 ▼
Password
 │
 ▼
passwd
 │
 ▼
Hash + Salt
 │
 ▼
/etc/shadow
 │
 ▼
PAM
 │
 ▼
Authentication
 │
 ▼
Access Granted
```

---

# 📚 Next File

Continue with:

```text
sudo.md
```

You will learn:

✅ How sudo actually works internally

✅ Difference between root and sudo

✅ Privilege escalation

✅ Security boundaries

✅ sudo token caching

✅ Attack vectors

✅ Enterprise access control models

✅ Why modern Linux prefers sudo over root login
