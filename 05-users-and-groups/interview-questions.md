# 🎯 Linux Users & Groups — Interview Questions (Beginner to Expert)

> This document is designed to build deep understanding, not just help you pass interviews. Every question is chosen because it teaches an important Linux concept, security principle, architecture decision, or real-world troubleshooting scenario.

---

# 🟢 Level 1 — Beginner Fundamentals

---

## Q1. What is a user in Linux?

### Answer

A user represents an identity that can:

```text
Login
Own Files
Run Processes
Receive Permissions
```

Examples:

```text
rahul
root
nginx
mysql
```

---

## Q2. Why does Linux need users?

### Answer

Without users:

```text
No Accountability
No Isolation
No Security
```

Linux must know:

```text
Who created file?
Who deleted file?
Who ran command?
```

---

## Q3. What is the root user?

### Answer

Special account:

```text
UID = 0
```

Root bypasses most permission checks.

Visual:

```text
Normal User
     │
     ▼
Limited Permissions

Root
     │
     ▼
Unlimited Permissions
```

---

## Q4. What is UID?

### Answer

UID = User Identifier

Linux trusts:

```text
UID
```

more than:

```text
Username
```

---

## Interview Trap

Question:

```text
Can two users have same username?
```

Normally no.

---

Question:

```text
Can two users have same UID?
```

Technically yes.

Dangerous.

---

## Q5. Why is UID more important than username?

Because file ownership is stored using:

```text
UID
```

not username.

---

# 🟡 Level 2 — Groups

---

## Q6. Why do groups exist?

Without groups:

```text
Assign Permissions
To Every User Individually
```

Nightmare.

Groups simplify management.

---

Visual

```text
Users
 │
 ▼
Group
 │
 ▼
Permissions
```

---

## Q7. Difference between primary and secondary groups?

Primary:

```text
Stored in /etc/passwd
```

Secondary:

```text
Stored in /etc/group
```

---

## Interview Trap

Question:

```text
If user isn't listed in /etc/group,
can they still belong to group?
```

Yes.

As primary group.

---

## Q8. Why are groups important in enterprises?

Imagine:

```text
500 Developers
```

Instead of:

```text
500 Permission Rules
```

Use:

```text
1 Group
```

---

# 🟠 Level 3 — Identity Files

---

## Q9. Difference between:

```text
/etc/passwd
```

and

```text
/etc/shadow
```

---

### /etc/passwd

Stores:

```text
Username
UID
GID
Shell
Home Directory
```

---

### /etc/shadow

Stores:

```text
Password Hashes
Password Aging
Expiration Policies
```

---

## Q10. Why aren't passwords stored in /etc/passwd?

Because:

```text
/etc/passwd
```

must be readable by many programs.

Storing hashes there increases attack surface.

---

## Q11. Why was /etc/shadow introduced?

To protect password hashes.

---

# 🔴 Level 4 — Authentication Concepts

---

## Q12. Difference between authentication and authorization?

---

Authentication:

```text
Who are you?
```

---

Authorization:

```text
What are you allowed to do?
```

---

Visual

```text
Authentication
      │
      ▼
Identity Verified
      │
      ▼
Authorization
      │
      ▼
Permission Granted
```

---

## Q13. Can authentication succeed but authorization fail?

Yes.

Example:

```text
Login Success
```

but:

```text
Permission Denied
```

for certain operations.

---

## Q14. Why does Linux hash passwords?

Because passwords should never be stored directly.

---

## Q15. What is a salt?

Random data added before hashing.

Protects against:

```text
Rainbow Tables
Precomputed Attacks
```

---

## Interview Trap

Question:

```text
Can two users with same password
have different hashes?
```

Yes.

Because of salts.

---

# 🟣 Level 5 — sudo & Privilege Escalation

---

## Q16. Why does sudo exist?

To provide:

```text
Controlled Privilege Escalation
```

instead of sharing root password.

---

## Q17. Difference between root and sudo?

Root:

```text
Permanent Administrator
```

sudo:

```text
Temporary Administrator
```

---

## Q18. Why does sudo ask for my password instead of root password?

Because sudo verifies:

```text
Your Identity
```

not root's.

---

## Q19. What file controls sudo access?

```text
/etc/sudoers
```

and

```text
/etc/sudoers.d/
```

---

## Q20. Why should sudoers be edited with visudo?

Prevents:

```text
Syntax Errors
Broken sudo
Administrative Lockout
```

---

# ⚫ Level 6 — Deep Security Questions

---

## Q21. What is the Principle of Least Privilege?

Give:

```text
Minimum Required Access
```

Not:

```text
Maximum Possible Access
```

---

Visual

```text
Need
 │
 ▼
Minimum Permissions
```

---

## Q22. Why is:

```text
NOPASSWD: ALL
```

dangerous?

Compromised account:

```text
Instant Root Access
```

---

## Q23. Why are service accounts usually locked?

Examples:

```text
mysql
nginx
postgres
```

Should not allow:

```text
Human Login
```

---

## Q24. Why is UID reuse dangerous?

Example:

```text
Old Employee UID=1001
```

Files remain.

New employee receives:

```text
UID=1001
```

Unexpected ownership appears.

---

# 🟤 Level 7 — Scenario Questions

---

## Scenario 1

User deleted:

```bash
userdel rahul
```

Files still exist.

Question:

```text
Who owns files?
```

Answer:

```text
UID still owns them.
```

---

## Scenario 2

Account locked.

Question:

```text
Can user still access using SSH key?
```

Potentially yes.

Depends on configuration.

---

## Scenario 3

User removed from sudo group.

Question:

```text
Can active session still use sudo?
```

Possibly until:

```text
New Session
Credential Refresh
```

---

## Scenario 4

User cannot run:

```bash
sudo command
```

but belongs to sudo group.

What would you check?

Answer:

```text
sudoers
Group Membership
Session Refresh
PAM
Logs
```

---

# 🔵 Level 8 — Troubleshooting Questions

---

## Q25. User cannot login.

What would you investigate?

---

Checklist:

```text
Account Locked?
Password Expired?
Shell Valid?
Home Exists?
PAM?
SSH Configuration?
```

---

Visual

```text
Login Failure
      │
      ▼
Authentication?
      │
      ▼
Authorization?
      │
      ▼
Environment?
```

---

## Q26. User exists but login fails.

Possible causes?

```text
nologin Shell
Expired Password
Expired Account
Locked Account
PAM Restriction
```

---

## Q27. User can login but commands fail.

Possible causes?

```text
Broken PATH
Permissions
Shell Config
Environment Variables
```

---

# 🟠 Level 9 — Architecture Questions

---

## Q28. Why separate authentication and authorization?

Different responsibilities.

---

Authentication:

```text
Identity Verification
```

---

Authorization:

```text
Permission Decisions
```

---

## Q29. Why separate:

```text
passwd
shadow
group
```

files?

Security and scalability.

---

## Q30. Why is Linux built around users and groups?

Allows:

```text
Isolation
Multi-User Operation
Security
Accountability
```

---

# 🔴 Level 10 — Enterprise Questions

---

## Q31. Why do enterprises use LDAP?

Centralized identity management.

---

Visual

```text
Users
  │
  ▼
LDAP
  │
  ▼
1000 Servers
```

---

## Q32. Why integrate Linux with Active Directory?

Single identity source.

---

## Q33. Why lock accounts before deleting them?

Preserve:

```text
Evidence
Data
Audit Trails
```

---

## Q34. Why use groups instead of assigning permissions individually?

Scalability.

---

## Q35. Why disable root SSH login?

For:

```text
Accountability
Auditing
Security
```

---

# ⚠️ Trick Questions

---

## Q36.

Can user have:

```text
UID=0
```

without username:

```text
root
```

?

Answer:

Yes.

Kernel trusts UID.

---

## Q37.

Can user login without password?

Answer:

Yes.

Depending on:

```text
SSH Keys
Certificates
PAM
Configuration
```

---

## Q38.

Can locked user still own files?

Yes.

Ownership remains.

---

## Q39.

Can root read other users' files?

Usually yes.

---

## Q40.

Can user be authenticated but still denied access?

Absolutely.

Authorization may fail.

---

# 🧠 Expert-Level Thinking Questions

---

## Q41.

Why is security fundamentally an identity problem?

Because Linux first asks:

```text
Who are you?
```

before deciding:

```text
What can you do?
```

---

## Q42.

Why are passwords gradually being replaced?

Human weakness.

Examples:

```text
Reuse
Weak Passwords
Phishing
```

---

## Q43.

What is more dangerous?

```text
Compromised Password
```

or

```text
Compromised sudo Account
```

Usually:

```text
Compromised sudo Account
```

because privilege escalation becomes easier.

---

## Q44.

What is the difference between:

```text
Identity
Authentication
Authorization
Accounting
```

Explain all four.

---

## Q45.

What happens internally from:

```text
User Login
```

to

```text
Shell Prompt
```

Draw full flow.

---

# 🎯 Final Master Question

Explain the complete lifecycle of a Linux user:

```text
Creation
Authentication
Authorization
Environment Loading
Privilege Escalation
Account Locking
Deletion
```

including:

```text
UID
GID
Groups
Passwords
sudo
PAM
SSH
Environment Variables
Ownership
Security Implications
```

If you can answer this clearly:

```text
You understand Linux Users & Groups at an engineering level,
not just a command level.
```

---

# 🚀 Next Module

Continue with:

```text
06-permissions/
```

Because now you know:

```text
WHO?
```

The next module teaches:

```text
WHAT CAN THEY DO?
```

Which is the core of Linux security.
