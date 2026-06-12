# 🎨 Linux Users & Groups Visual Guide

> A visual-first guide to understanding Linux users, groups, authentication, authorization, sudo, PAM, identity management, and enterprise access control.

---

# 🗺️ 1. Big Picture

```text
                    Linux Identity System

                             │
                             ▼

                    ┌─────────────────┐
                    │      User       │
                    └─────────────────┘
                             │
                             ▼

                    ┌─────────────────┐
                    │ Authentication  │
                    └─────────────────┘
                             │
          ┌──────────────────┼──────────────────┐
          ▼                  ▼                  ▼

      Password          SSH Key             MFA

                             │
                             ▼

                    ┌─────────────────┐
                    │ Authorization   │
                    └─────────────────┘
                             │
            ┌────────────────┼────────────────┐
            ▼                ▼                ▼

          Users           Groups            sudo

                             │
                             ▼

                    ┌─────────────────┐
                    │ Permissions     │
                    └─────────────────┘
```

---

# 👤 2. User Lifecycle

```text
Create User
     │
     ▼
 useradd
     │
     ▼
Assign Password
     │
     ▼
 Assign Groups
     │
     ▼
 User Login
     │
     ▼
 Normal Usage
     │
     ▼
 Lock Account
     │
     ▼
 Delete User
```

---

# 🆔 3. Username vs UID

```text
Human Sees:

rahul
priya
aman

Kernel Sees:

1001
1002
1003
```

---

## Internal Mapping

```text
Username
    │
    ▼
 Lookup
    │
    ▼
 UID
    │
    ▼
 Kernel Trusts UID
```

---

# 📁 4. File Ownership

```text
File
 │
 ▼
report.txt
 │
 ▼
Owner UID = 1001
Group GID = 1001
```

---

## Ownership Lookup

```text
UID 1001
    │
    ▼
/etc/passwd
    │
    ▼
rahul
```

---

# 👥 5. Why Groups Exist

Without Groups:

```text
100 Users
   │
   ▼
100 Permission Rules
```

---

With Groups:

```text
100 Users
     │
     ▼
 Developers Group
     │
     ▼
 Single Permission Rule
```

---

# 👥 6. Primary vs Secondary Groups

```text
User: rahul

Primary Group
     │
     ▼
 developers

Secondary Groups
     │
     ├── docker
     ├── sudo
     └── git
```

---

# 📄 7. Linux Identity Files

```text
/etc/passwd
      │
      ▼
 User Information

/etc/shadow
      │
      ▼
 Password Hashes

/etc/group
      │
      ▼
 Group Membership
```

---

# 🔑 8. Authentication Flow

```text
User Login
     │
     ▼
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
 ┌────┴────┐
 │         │
Yes        No
 │          │
 ▼          ▼

Access    Denied
Granted
```

---

# 🧂 9. Password Hashing

```text
Password
    │
    ▼
 Add Salt
    │
    ▼
 Hash Function
    │
    ▼
 Store Hash
```

---

## Why Salt?

```text
User A Password123
User B Password123

Without Salt

Hash A = XYZ
Hash B = XYZ

With Salt

Hash A = ABC
Hash B = QWE
```

---

# 🔒 10. Account Locking

```text
User Exists
     │
     ▼
Password Locked
     │
     ▼
Cannot Login
```

---

## Lock vs Delete

```text
LOCK

User Exists
Files Exist
Permissions Exist

Login Disabled
```

---

```text
DELETE

User Removed
Authentication Removed
May Remove Data
```

---

# 🛡️ 11. Authentication vs Authorization

```text
Authentication
      │
      ▼
Who Are You?
      │
      ▼
Identity Verified
      │
      ▼
Authorization
      │
      ▼
What Can You Do?
```

---

# 🚀 12. sudo Architecture

```text
User
 │
 ▼
sudo command
 │
 ▼
sudoers Check
 │
 ▼
Authentication
 │
 ▼
Authorization
 │
 ▼
Run As Root
 │
 ▼
Log Action
```

---

# 🔥 13. Root vs sudo

```text
Root Login

User
 │
 ▼
Permanent Root
 │
 ▼
Everything
```

---

```text
sudo

User
 │
 ▼
Temporary Privilege
 │
 ▼
Specific Task
```

---

# 📜 14. sudoers Flow

```text
sudo
 │
 ▼
/etc/sudoers
 │
 ▼
Permission Rule
 │
 ├── Allow
 │
 └── Deny
```

---

# 🏢 15. Enterprise Authentication

```text
Employee
    │
    ▼
Linux Server
    │
    ▼
SSSD
    │
    ▼
LDAP / AD
    │
    ▼
Authentication Result
```

---

# 🌐 16. Active Directory Integration

```text
Employees
      │
      ▼
Active Directory
      │
      ▼
Linux Servers
      │
      ▼
Single Identity Source
```

---

# 🎫 17. Kerberos Authentication

```text
User
 │
 ▼
Password
 │
 ▼
Kerberos
 │
 ▼
TGT
 │
 ▼
Service Ticket
 │
 ▼
Access Granted
```

---

# 🔌 18. PAM Architecture

```text
Login Request
      │
      ▼
PAM
      │
 ┌────┼────┐
 ▼    ▼    ▼

Auth Account Session
```

---

## PAM Decision Tree

```text
User Login
     │
     ▼
PAM Auth
     │
     ▼
Password Valid?
     │
 ┌───┴───┐
 │       │

No      Yes
 │       │
 ▼       ▼

Deny   Continue
```

---

# ☁️ 19. Linux vs Cloud IAM

```text
Linux

User
 │
 ▼
UID
 │
 ▼
Permissions
```

---

```text
Cloud

Identity
    │
    ▼
Role
    │
    ▼
Policy
    │
    ▼
Access
```

---

# ☸️ 20. Linux vs Kubernetes RBAC

```text
Linux

User
 │
 ▼
Group
 │
 ▼
Permissions
```

---

```text
Kubernetes

User
 │
 ▼
Role
 │
 ▼
RoleBinding
 │
 ▼
Permissions
```

---

# 🔐 21. Zero Trust Model

Traditional:

```text
Login Once
     │
     ▼
Trust Forever
```

---

Zero Trust:

```text
Request
   │
   ▼
Verify
   │
   ▼
Allow
   │
   ▼
Verify Again
   │
   ▼
Allow
```

---

# 🏗️ 22. Complete Enterprise Identity Flow

```text
Employee
    │
    ▼
Identity Provider
    │
    ▼
Authentication
    │
    ▼
LDAP / AD
    │
    ▼
Linux Login
    │
    ▼
PAM
    │
    ▼
Authorization
    │
    ▼
sudo
    │
    ▼
Production Access
    │
    ▼
Audit Logs
```

---

# 🔥 23. Attack Chain Visualization

```text
Compromised Password
         │
         ▼
User Account
         │
         ▼
sudo Access?
         │
    ┌────┴────┐
    │         │

   No        Yes
    │         │
    ▼         ▼

 Limited    Root Access
 Impact
```

---

# 🛡️ 24. Defense in Depth

```text
Attacker
    │
    ▼

Password
    │
    ▼

MFA
    │
    ▼

PAM Rules
    │
    ▼

sudo Restrictions
    │
    ▼

SELinux
    │
    ▼

Audit Logs
```

---

# 🎯 25. Everything Together

```text
                Linux Identity Ecosystem

                        User
                         │
                         ▼

                 Authentication
                         │

      ┌──────────────────┼──────────────────┐
      ▼                  ▼                  ▼

  Password           SSH Key             MFA

                         │
                         ▼

                       PAM
                         │
                         ▼

                  Authorization
                         │

         ┌───────────────┼───────────────┐
         ▼               ▼               ▼

       User           Groups           sudo

                         │
                         ▼

                    Permissions
                         │
                         ▼

                     Resources
                         │
                         ▼

                   Audit Logging
                         │
                         ▼

                    Security
```

---

# 🚀 Mental Model

Remember Linux Security as:

```text
WHO?
 │
 ▼
Users & Groups

HOW DO WE VERIFY?
 │
 ▼
Authentication

WHAT CAN THEY DO?
 │
 ▼
Authorization

HOW IS IT ENFORCED?
 │
 ▼
Permissions

HOW IS IT TRACKED?
 │
 ▼
Auditing
```

If you understand this diagram, you understand the entire `05-users-and-groups` module.
