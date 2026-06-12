# 🔐 Understanding `/etc/shadow` in Linux

> Learn how Linux securely stores passwords, manages password expiration, controls account aging, and protects user authentication using the **`/etc/shadow`** file.

---

# 🎯 What is `/etc/shadow`?

Imagine a school has two records:

### Public Student Register

Everyone can see:

```text
Student Name
Roll Number
Class
```

Stored in:

```text
/etc/passwd
```

---

### Secret Security Register

Only authorized staff can see:

```text
Passwords
Account Expiry Dates
Password Policies
```

Stored in:

```text
/etc/shadow
```

---

# 🧠 Why Does `/etc/shadow` Exist?

Early Unix systems stored encrypted passwords in:

```text
/etc/passwd
```

Problem:

```text
Everyone could read it
```

Attackers could copy password hashes and attempt:

```text
Brute Force Attacks
Dictionary Attacks
Offline Password Cracking
```

To solve this:

```text
User Information  → /etc/passwd
Password Data     → /etc/shadow
```

Linux separated authentication data from user information.

---

# 🔒 Main Purpose

`/etc/shadow` stores:

```text
Password Hashes
Password Aging Rules
Account Expiration
Password Expiration
Warning Periods
Inactive Account Settings
```

---

# 📍 Location

```text
/etc/shadow
```

Verify:

```bash
ls -l /etc/shadow
```

Example:

```text
-r-------- 1 root shadow 1800 Jun 12 12:00 /etc/shadow
```

Notice:

```text
Only root can read it
```

Unlike:

```text
/etc/passwd
```

which is readable by everyone.

---

# 🎨 Permission Comparison

```text
/etc/passwd

-rw-r--r--

Everyone can read
```

---

```text
/etc/shadow

-r-------- 

Only root can read
```

Visual:

```text
/etc/passwd

🔓 Public

Everyone
   │
   ▼
 Can Read
```

---

```text
/etc/shadow

🔒 Protected

Root
  │
  ▼
Can Read
```

---

# 📋 Viewing the File

Root required:

```bash
sudo cat /etc/shadow
```

Example:

```text
rahul:$6$AbC123xyz$R5k9....:19880:0:99999:7:::
```

Each line belongs to one user.

---

# 🏗️ Structure of an Entry

General format:

```text
username:password:lastchange:min:max:warn:inactive:expire:reserved
```

Visual:

```text
┌─────┬─────────┬────────┬────┬────┬────┬────────┬────────┬─────────┐
│user │password │change  │min │max │warn│inactive│expire │reserved │
└─────┴─────────┴────────┴────┴────┴────┴────────┴────────┴─────────┘
```

---

# Example Entry

```text
rahul:$6$abcxyz$7fjH...:19880:0:99999:7:::
```

Let's decode every field.

---

# 🔍 Field 1 — Username

Example:

```text
rahul
```

Must match:

```text
/etc/passwd
```

Visual:

```text
/etc/passwd
      │
      ▼
rahul
      ▲
      │
/etc/shadow
```

Both files work together.

---

# 🔍 Field 2 — Password Hash

Example:

```text
$6$abcxyz$7fjH...
```

This is NOT the password.

It is:

```text
Password Hash
```

---

# What Is a Hash?

Imagine:

Password:

```text
mypassword123
```

Linux converts it into:

```text
f8b8d9f9a72c7e...
```

using a cryptographic algorithm.

Visual:

```text
Password
    │
    ▼
Hash Function
    │
    ▼
Hash Value
```

---

# Why Hash Passwords?

Linux should never store:

```text
mypassword123
```

Directly.

Instead:

```text
$6$abcxyz$7fjH...
```

Even administrators cannot easily reverse it.

---

# Hash Algorithms

Common identifiers:

| Prefix | Algorithm |
| ------ | --------- |
| $1$    | MD5       |
| $2a$   | Blowfish  |
| $5$    | SHA-256   |
| $6$    | SHA-512   |

Example:

```text
$6$
```

means:

```text
SHA-512
```

---

# Password Verification Process

When logging in:

```text
Enter Password
       │
       ▼
Hash Password
       │
       ▼
Compare Hash
       │
       ▼
Match?
```

If:

```text
Hashes Match
```

Login succeeds.

---

# 🔍 Special Password Values

---

## x

Usually found in:

```text
/etc/passwd
```

Meaning:

```text
Password stored in shadow
```

---

## !

Example:

```text
rahul:!:19880:0:99999:7:::
```

Meaning:

```text
Account Locked
```

Visual:

```text
Login Attempt
      │
      ▼
Locked Account
      │
      ▼
Access Denied
```

---

## *

Example:

```text
mysql:*:19880:0:99999:7:::
```

Meaning:

```text
No Password Login Allowed
```

Common for system accounts.

---

# 🔍 Field 3 — Last Password Change

Example:

```text
19880
```

This is:

```text
Days since
January 1, 1970
```

called:

```text
Unix Epoch
```

Linux uses this to track password age.

---

# 🔍 Field 4 — Minimum Password Age

Example:

```text
0
```

Meaning:

```text
Password can be changed immediately
```

Example:

```text
7
```

Means:

```text
Must wait 7 days before changing password
```

---

# Why Use It?

Prevents users from:

```text
Password1
Password2
Password3
Password1
```

cycling through passwords quickly.

---

# 🔍 Field 5 — Maximum Password Age

Example:

```text
90
```

Meaning:

```text
Password expires after 90 days
```

User must create a new password.

Visual:

```text
Password Created
        │
        ▼
90 Days
        │
        ▼
Expired
```

---

# 🔍 Field 6 — Warning Period

Example:

```text
7
```

Means:

```text
Warn user 7 days before expiration
```

Visual:

```text
Day 83
   │
   ▼
⚠ Password expires soon
```

---

# 🔍 Field 7 — Inactive Period

Example:

```text
30
```

Meaning:

```text
After password expiration,
account disabled after 30 days
```

Visual:

```text
Password Expires
       │
       ▼
30 Days Pass
       │
       ▼
Account Disabled
```

---

# 🔍 Field 8 — Account Expiration Date

Example:

```text
21000
```

Represents:

```text
Specific future date
```

After that:

```text
Account Disabled
```

regardless of password status.

Useful for:

```text
Contract Employees
Interns
Temporary Accounts
```

---

# 🔍 Field 9 — Reserved

Usually empty.

Reserved for future use.

Example:

```text
:
```

---

# 🏗️ Complete Entry Breakdown

Example:

```text
rahul:$6$abcxyz$7fjH...:19880:7:90:14:30:
```

Meaning:

```text
Username           : rahul
Hash               : SHA-512 hash
Last Changed       : Day 19880
Min Age            : 7 Days
Max Age            : 90 Days
Warning            : 14 Days
Inactive           : 30 Days
Expiration         : Not Set
```

---

# 🔍 Viewing Password Aging

Use:

```bash
sudo chage -l rahul
```

Example:

```text
Last password change
Password expires
Password inactive
Account expires
```

This is the easiest way to read shadow information.

---

# ⚙️ Managing Password Policies

---

## Set Maximum Password Age

```bash
sudo chage -M 90 rahul
```

Password expires after:

```text
90 days
```

---

## Set Minimum Age

```bash
sudo chage -m 7 rahul
```

Must wait:

```text
7 days
```

before changing password again.

---

## Set Warning Days

```bash
sudo chage -W 14 rahul
```

Warn:

```text
14 days before expiration
```

---

## Set Account Expiry

```bash
sudo chage -E 2027-12-31 rahul
```

Account disabled after:

```text
31 Dec 2027
```

---

# 🔒 Locking Accounts

Lock:

```bash
sudo passwd -l rahul
```

Linux adds:

```text
!
```

to the hash field.

Example:

Before:

```text
$6$abcxyz$...
```

After:

```text
!$6$abcxyz$...
```

Login blocked.

---

# 🔓 Unlocking Accounts

```bash
sudo passwd -u rahul
```

Restores login capability.

---

# 🏢 Real-World Enterprise Policies

Many organizations enforce:

```text
Minimum Age      : 1 Day
Maximum Age      : 90 Days
Warning Period   : 14 Days
Account Lockout  : Enabled
```

Benefits:

```text
Security
Compliance
Auditing
Risk Reduction
```

---

# 🚨 Common Mistakes

---

## Editing Directly

Bad:

```bash
nano /etc/shadow
```

Can corrupt authentication database.

Use:

```bash
sudo vipw -s
```

---

## Weak Password Policies

Bad:

```text
Never Expire Passwords
```

Risk:

```text
Credential Theft
Long-Term Compromise
```

---

## World Readable Shadow File

Very dangerous.

Verify:

```bash
ls -l /etc/shadow
```

Should remain root-only.

---

# 🔄 Login Authentication Flow

```text
User Login
     │
     ▼
Enter Username
     │
     ▼
Check /etc/passwd
     │
     ▼
Locate User
     │
     ▼
Check /etc/shadow
     │
     ▼
Read Hash
     │
     ▼
Hash Input Password
     │
     ▼
Compare Hashes
     │
     ▼
Success or Failure
```

---

# ⚡ Useful Commands Cheat Sheet

Show shadow entry:

```bash
sudo grep rahul /etc/shadow
```

---

View aging:

```bash
sudo chage -l rahul
```

---

Set max age:

```bash
sudo chage -M 90 rahul
```

---

Set min age:

```bash
sudo chage -m 7 rahul
```

---

Set warning:

```bash
sudo chage -W 14 rahul
```

---

Lock account:

```bash
sudo passwd -l rahul
```

---

Unlock account:

```bash
sudo passwd -u rahul
```

---

Safely edit:

```bash
sudo vipw -s
```

---

# 🧠 Interview Questions

### What is `/etc/shadow`?

Stores password hashes and password-aging information.

---

### Why was `/etc/shadow` introduced?

To protect password hashes from being readable by all users.

---

### What does `$6$` mean?

```text
SHA-512 Hash Algorithm
```

---

### What does `!` in the password field mean?

Account is locked.

---

### What command displays password aging information?

```bash
chage -l username
```

---

### Which file stores actual password hashes?

```text
/etc/shadow
```

---

### Difference between `/etc/passwd` and `/etc/shadow`?

```text
/etc/passwd
    User Information

/etc/shadow
    Authentication Information
```

---

### How do you safely edit shadow?

```bash
vipw -s
```

---

# 🎯 Summary

`/etc/shadow` is Linux's secure authentication database.

It stores:

```text
Password Hashes
Password Aging
Password Expiration
Account Expiration
Security Policies
```

It protects systems by:

```text
Separating Passwords
Enforcing Policies
Supporting Secure Authentication
Preventing Unauthorized Access
```

Understanding `/etc/shadow` is essential for:

```text
Linux Administration
DevOps
Cybersecurity
Cloud Engineering
System Hardening
```

---

# 📚 Next File

Continue with:

```text
group-file.md
```

You will learn:

✅ Structure of `/etc/group`

✅ Group IDs (GIDs)

✅ Group membership management

✅ Primary vs secondary groups

✅ How Linux resolves group permissions
