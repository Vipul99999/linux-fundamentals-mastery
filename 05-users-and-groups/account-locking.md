# 🔒 Account Locking in Linux — Complete Beginner to Incident Response Guide

> Learn what account locking really means, how Linux disables access, the difference between password locking and account locking, security incident workflows, enterprise offboarding, attack prevention, forensic considerations, and advanced authentication edge cases.

---

# 🎯 What is Account Locking?

Imagine a school.

A student normally enters using an ID card.

```text
Student
   │
   ▼
ID Card
   │
   ▼
School Entry
```

Now suppose:

```text
Student Violates Rules
Account Suspicious
Investigation Required
Contract Ended
```

The school may temporarily disable access.

The student record still exists.

But entry is denied.

Linux account locking works exactly the same way.

---

# 🧠 Simple Definition

Account locking means:

```text
User Account Exists
Data Exists
Permissions Exist
Login Blocked
```

---

# Visual

```text
User Account
      │
      ▼
LOCK
      │
      ▼
Cannot Login
```

---

# 🚨 Biggest Beginner Misconception

Many people think:

```text
Lock Account
      │
      ▼
Delete Account
```

Wrong.

Locking is NOT deletion.

---

# Visual

```text
Locking

User Exists
Files Exist
Groups Exist
Login Disabled
```

---

```text
Deletion

User Removed
Authentication Removed
May Remove Data
```

---

# Why Lock Instead of Delete?

Many reasons.

---

# Security Investigation

Suppose:

```text
Suspicious Login
Possible Breach
Insider Threat
```

Immediately deleting account destroys evidence.

Better:

```text
Lock Account
Preserve Data
Investigate
```

---

# Employee Leaving Company

Instead of:

```bash
userdel employee
```

Immediately.

Most enterprises:

```text
Disable Access
Archive Data
Review Ownership
Delete Later
```

---

# Incident Response

If attacker compromises:

```text
Rahul Account
```

Security team usually:

```text
Lock First
Investigate Second
```

---

# Linux Authentication Overview

Before understanding locking, understand login.

---

# Visual

```text
User Login
      │
      ▼
Username
      │
      ▼
Password Check
      │
      ▼
Authentication
      │
      ▼
Shell Access
```

---

Locking interrupts this process.

---

# Different Types of Locking

Most tutorials discuss only one.

Linux actually has several.

---

# 1. Password Locking

Most common.

Blocks:

```text
Password Authentication
```

---

# 2. Account Expiration

Blocks:

```text
Entire Account
```

after specified date.

---

# 3. Shell Locking

Blocks:

```text
Interactive Login
```

through shell changes.

---

# 4. PAM Restrictions

Blocks:

```text
Authentication Layer
```

access.

---

# 5. SSH Restrictions

Blocks:

```text
Remote Access
```

only.

---

# Understanding Password Locking

Command:

```bash
sudo passwd -l rahul
```

or

```bash
sudo usermod -L rahul
```

---

# What Happens Internally?

Linux modifies:

```text
/etc/shadow
```

Before:

```text
rahul:$6$ABCDEF...
```

After:

```text
rahul:!$6$ABCDEF...
```

---

# Visual

```text
Password Hash
      │
      ▼
Add !
      │
      ▼
Password Authentication Disabled
```

---

# Why Does ! Matter?

Linux sees:

```text
!hash
```

and understands:

```text
Password Cannot Match
```

---

# Important Edge Case

Question:

```text
Can locked account still access through SSH keys?
```

Answer:

```text
Sometimes Yes
```

depending on configuration.

This surprises many administrators.

---

# Visual

```text
Password Locked
      │
      ▼
SSH Key Login?
      │
      ▼
Maybe
```

---

# Security Lesson

Password lock does NOT necessarily mean:

```text
Complete Access Block
```

---

# Unlocking Accounts

Command:

```bash
sudo passwd -u rahul
```

or

```bash
sudo usermod -U rahul
```

---

# Visual

```text
Locked
   │
   ▼
Unlock
   │
   ▼
Login Allowed
```

---

# Checking Account Status

Command:

```bash
passwd -S rahul
```

Example:

```text
rahul L
```

Meaning:

```text
Locked
```

---

Other values:

```text
P = Password Set
L = Locked
NP = No Password
```

---

# Visual

```text
P
│
└── Active

L
│
└── Locked

NP
│
└── No Password
```

---

# Account Expiration

Different from locking.

Example:

```bash
sudo usermod -e 2026-12-31 rahul
```

---

# Visual

```text
Account Created
       │
       ▼
31 Dec 2026
       │
       ▼
Account Disabled
```

---

# Why Use Expiration?

Useful for:

```text
Interns
Consultants
Contractors
Temporary Access
Training Accounts
```

---

# Enterprise Example

Contractor hired:

```text
1 January
```

Access automatically ends:

```text
31 March
```

No manual intervention required.

---

# Shell-Based Locking

Change shell:

```bash
sudo usermod -s /usr/sbin/nologin rahul
```

---

# Visual

```text
Login
  │
  ▼
nologin
  │
  ▼
Access Denied
```

---

# Why Use This?

Common for:

```text
Service Accounts
Application Accounts
Automation Accounts
```

Examples:

```text
mysql
nginx
postgres
```

---

# Important Concept

Service accounts usually should NOT allow:

```text
Interactive Login
```

---

# SSH-Specific Locking

Suppose:

```text
User Allowed Local Login
```

but:

```text
SSH Must Be Disabled
```

Possible using:

```text
AllowUsers
DenyUsers
Match Blocks
```

inside:

```text
/etc/ssh/sshd_config
```

---

# Visual

```text
Local Login
     │
     ▼
Allowed

SSH Login
     │
     ▼
Denied
```

---

# PAM-Based Locking

Advanced systems often use:

```text
PAM
```

to restrict access.

---

# What is PAM?

```text
Pluggable Authentication Modules
```

---

# Visual

```text
Login
  │
  ▼
PAM
  │
  ▼
Allow / Deny
```

---

# Why PAM Matters

Can enforce:

```text
Time Restrictions
Location Restrictions
Group Restrictions
Security Policies
```

---

# Example

Only allow login:

```text
9 AM - 6 PM
```

using PAM policies.

---

# Failed Login Protection

Question:

```text
What if attacker tries 10000 passwords?
```

Linux can lock account after repeated failures.

---

# Visual

```text
Wrong Password
      │
      ▼
Wrong Password
      │
      ▼
Wrong Password
      │
      ▼
Account Locked
```

---

# Common Tools

```text
pam_faillock
pam_tally2
faillock
```

---

# Enterprise Security Workflow

Typical enterprise policy:

```text
5 Failed Attempts
       │
       ▼
15 Minute Lockout
```

---

# Why?

Protects against:

```text
Brute Force
Password Spraying
Credential Stuffing
```

---

# Security Incident Workflow

Account compromised.

---

# Wrong Approach

```text
Delete User Immediately
```

Problems:

```text
Destroy Evidence
Lose Audit Trail
Complicate Investigation
```

---

# Better Approach

```text
Lock Account
Collect Logs
Analyze Activity
Rotate Credentials
Review Access
```

---

# Visual

```text
Compromise Detected
        │
        ▼
Lock Account
        │
        ▼
Investigation
        │
        ▼
Remediation
```

---

# Employee Offboarding Workflow

Employee leaves company.

---

# Typical Process

```text
Disable Account
Lock Password
Archive Mail
Archive Files
Review Ownership
Remove Access
Delete Later
```

---

# Why Not Delete Immediately?

Because organization may need:

```text
Emails
Documents
Logs
Audit Records
Evidence
```

---

# Shared System Edge Case

Suppose:

```text
University Lab
Research Cluster
Shared Server
```

Account locked.

Question:

```text
Should files be removed?
```

Not always.

Other users may still depend on:

```text
Shared Data
Research Results
Project Files
```

---

# Deep Security Concept

Locking ≠ Authorization Removal

---

Example:

User belongs to:

```text
sudo
docker
developers
```

Groups remain.

Only authentication blocked.

---

# Visual

```text
Authentication
      │
      ▼
LOCKED

Authorization
      │
      ▼
Still Exists
```

---

# Why Is This Important?

If account later unlocked:

```text
All Previous Permissions Return
```

Immediately.

---

# Dangerous Assumption

Wrong:

```text
Locked User Is Harmless
```

Potential risks:

```text
Running Processes
SSH Keys
API Tokens
Certificates
Cron Jobs
```

may still exist.

---

# Running Process Edge Case

User locked.

Question:

```text
Do existing processes stop?
```

Answer:

```text
No
```

Usually they continue.

---

# Visual

```text
User Locked
      │
      ▼
Process Still Running
```

---

# Example

```bash
ps -u rahul
```

may still show:

```text
python app.py
backup.sh
```

---

# Token-Based Authentication Edge Case

Modern systems may use:

```text
OAuth Tokens
Kerberos Tickets
API Tokens
Cloud Credentials
```

Locking password alone may not revoke them.

---

# Enterprise Response

Must also:

```text
Revoke Tokens
Invalidate Sessions
Rotate Credentials
```

---

# Containers and Kubernetes

Question:

```text
Does account locking matter inside containers?
```

Often:

```text
Less Important
```

because containers may not use traditional logins.

Still relevant for:

```text
Interactive Access
Debug Containers
Shared Systems
```

---

# Useful Commands

Lock account:

```bash
sudo passwd -l username
```

---

Unlock account:

```bash
sudo passwd -u username
```

---

Alternative lock:

```bash
sudo usermod -L username
```

---

Alternative unlock:

```bash
sudo usermod -U username
```

---

Set expiration:

```bash
sudo usermod -e YYYY-MM-DD username
```

---

View aging:

```bash
sudo chage -l username
```

---

Check status:

```bash
passwd -S username
```

---

List active processes:

```bash
ps -u username
```

---

# Questions Most Tutorials Never Answer

### Is locking the same as deleting?

No.

---

### Is password locking the same as account locking?

Not always.

Password authentication may be blocked while other methods still work.

---

### Can locked users still own files?

Yes.

Ownership remains.

---

### Can locked users still have running processes?

Yes.

Very common.

---

### Can locked users still have valid SSH keys?

Possibly.

Depends on configuration.

---

### Why do enterprises lock before deleting?

Preserve evidence and business data.

---

### Does locking remove sudo access?

No.

Permissions remain attached to account.

Authentication is what gets blocked.

---

### Can account expiration replace locking?

Sometimes.

But they solve different problems.

---

### Why are service accounts often locked?

Prevent human logins.

---

# 🧠 Interview Deep-Dive Questions

### Difference between:

```bash
passwd -l
```

and

```bash
userdel
```

---

### Difference between:

```bash
passwd -l
```

and

```bash
usermod -e
```

---

### Why might a locked user still access a system?

Through:

```text
SSH Keys
Tokens
Running Sessions
Certificates
```

---

### What should security teams do after locking a compromised account?

```text
Review Logs
Terminate Sessions
Revoke Tokens
Rotate Credentials
```

---

### Why are service accounts commonly assigned:

```text
/usr/sbin/nologin
```

?

To prevent interactive login.

---

# 🎯 Summary

Account locking is one of Linux's most important security controls.

It helps with:

```text
Incident Response
Offboarding
Compliance
Access Control
Attack Mitigation
Authentication Management
```

Understanding account locking means understanding:

```text
Authentication
Authorization
Sessions
Tokens
SSH
PAM
Identity Lifecycle Management
```

---

# 🗺️ Big Picture

```text
User
 │
 ▼
Authentication
 │
 ▼
Password / SSH / PAM
 │
 ▼
Account Lock
 │
 ▼
Access Denied
 │
 ▼
Investigation / Review
 │
 ▼
Unlock or Delete
```

---

# 📚 Next File

Continue with:

```text
user-environment.md
```

You will learn:

✅ What happens after login

✅ Shell initialization process

✅ Environment variables

✅ PATH internals

✅ Profile files

✅ Bash startup sequence

✅ Security risks of environment variables

✅ Real-world debugging of login environments

✅ Enterprise environment management
