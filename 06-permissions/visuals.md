# Linux Permissions Visual Guide

> A visual reference guide for understanding Linux permissions, ownership, ACLs, SUID, SGID, Sticky Bit, Capabilities, SELinux, AppArmor, and permission troubleshooting.

Use this file as:

```text
Quick Revision Sheet

Interview Cheat Sheet

Troubleshooting Reference

Mental Model Guide
```

---

# 1. Linux Security Layers

High-Level View

```text
+------------------------------------------------+
|                Applications                    |
+------------------------------------------------+
                        │
                        ▼
+------------------------------------------------+
|                Ownership                        |
+------------------------------------------------+
                        │
                        ▼
+------------------------------------------------+
|          Traditional Permissions               |
|              (rwx rwx rwx)                     |
+------------------------------------------------+
                        │
                        ▼
+------------------------------------------------+
|                     ACL                         |
+------------------------------------------------+
                        │
                        ▼
+------------------------------------------------+
|                Capabilities                     |
+------------------------------------------------+
                        │
                        ▼
+------------------------------------------------+
|             SELinux / AppArmor                  |
+------------------------------------------------+
                        │
                        ▼
+------------------------------------------------+
|                Linux Kernel                     |
+------------------------------------------------+
```

---

# 2. Ownership Model

Every file has:

```text
Owner

Group

Others
```

Visualization:

```text
file.txt

      Owner
        │
        ▼
      alice

      Group
        │
        ▼
   developers

      Others
        │
        ▼
 Everyone Else
```

---

# 3. Permission Structure

Example:

```text
-rwxr-x---
```

Breakdown:

```text
     Owner  Group Others
       │      │      │
       ▼      ▼      ▼
       rwx    r-x    ---
```

---

# Binary Representation

```text
r = 4

w = 2

x = 1
```

---

Visualization

```text
7 = 4+2+1 = rwx

6 = 4+2   = rw-

5 = 4+1   = r-x

4 = 4     = r--

0 = ---   = 0
```

---

# Permission Table

```text
+-----+---------+
| Num | Perms   |
+-----+---------+
| 777 | rwxrwxrwx |
| 755 | rwxr-xr-x |
| 750 | rwxr-x--- |
| 700 | rwx------ |
| 644 | rw-r--r-- |
| 640 | rw-r----- |
| 600 | rw------- |
+-----+---------+
```

---

# 4. Access Evaluation Flow

Linux decides access like this:

```text
User Requests Access
          │
          ▼
     Owner?
          │
     ┌────┴────┐
     │         │
    YES       NO
     │         │
     ▼         ▼
 Owner Bits  Group Check
                 │
                 ▼
          Group Member?
                 │
           ┌─────┴─────┐
           │           │
          YES         NO
           │           │
           ▼           ▼
      Group Bits   Others Bits
```

---

# 5. Directory Permission Mental Model

Files:

```text
Read    -> View Content

Write   -> Modify Content

Execute -> Run Program
```

---

Directories:

```text
Read    -> List Names

Write   -> Create/Delete

Execute -> Enter Directory
```

---

Visualization

```text
Directory

Read
 │
 ▼
See Files

Write
 │
 ▼
Create/Delete

Execute
 │
 ▼
Enter Directory
```

---

# 6. Directory Traversal

Path:

```text
/home/alice/docs/report.txt
```

Requirements:

```text
/
home
alice
docs
```

must all allow:

```text
Execute
```

---

Visualization

```text
/
│
▼
home
│
▼
alice
│
▼
docs
│
▼
report.txt
```

One missing execute bit:

```text
ACCESS DENIED
```

---

# 7. File Creation Process

When a file is created:

```text
Application
      │
      ▼
Create File
      │
      ▼
Default Permission
      │
      ▼
Apply umask
      │
      ▼
Final Permission
```

---

Files

```text
666
```

start here.

Directories

```text
777
```

start here.

---

Example

```text
666

umask 022

↓

644
```

---

Visualization

```text
666
 │
 ▼
022
 │
 ▼
644
```

---

# 8. SUID Architecture

Normal Program

```text
alice
 │
 ▼
program
 │
 ▼
EUID = alice
```

---

SUID Program

```text
alice
 │
 ▼
passwd
 │
 ▼
EUID = root
```

---

Kernel Flow

```text
Execute Program
       │
       ▼
SUID Bit?
       │
   ┌───┴───┐
   │       │
  No      Yes
   │       │
   ▼       ▼
Normal   Owner UID
```

---

# SUID Permission Layout

```text
-rwsr-xr-x
```

Visualization

```text
-rwsr-xr-x
   ▲
   │
 SUID
```

---

# 9. SGID Architecture

Without SGID

```text
Alice
 │
 ▼
frontend
```

```text
Bob
 │
 ▼
backend
```

---

With SGID

```text
Alice
 │
 ▼
developers
```

```text
Bob
 │
 ▼
developers
```

---

Directory Flow

```text
Create File
      │
      ▼
SGID Set?
      │
   ┌──┴──┐
   │     │
  No    Yes
   │     │
   ▼     ▼
User   Directory
Group  Group
```

---

# SGID Layout

```text
drwxr-sr-x
```

Visualization

```text
drwxr-sr-x
      ▲
      │
     SGID
```

---

# 10. Sticky Bit Architecture

Without Sticky Bit

```text
Alice File
     │
     ▼
Bob Deletes
     │
     ▼
SUCCESS
```

---

With Sticky Bit

```text
Alice File
     │
     ▼
Bob Deletes
     │
     ▼
DENIED
```

---

Deletion Flow

```text
Delete Request
       │
       ▼
Sticky Bit?
       │
   ┌───┴───┐
   │       │
  No      Yes
   │       │
   ▼       ▼
Allow  Ownership Check
```

---

Sticky Bit Layout

```text
drwxrwxrwt
```

Visualization

```text
drwxrwxrwt
          ▲
          │
      Sticky Bit
```

---

# 11. ACL Architecture

Traditional Model

```text
Owner

Group

Others
```

---

ACL Model

```text
Owner

Bob

Charlie

Finance Team

QA Team

Group

Others
```

---

Visualization

```text
File
 │
 ▼

Owner

Named Users

Named Groups

Mask

Others
```

---

ACL Evaluation

```text
Request
   │
   ▼
Owner?
   │
   ▼
Named User ACL?
   │
   ▼
Mask Applied
   │
   ▼
Group ACL?
   │
   ▼
Others
```

---

ACL Mask

```text
Requested:
rwx

Mask:
r--

Result:
r--
```

---

# 12. Capability Architecture

Traditional Linux

```text
Root
 │
 ▼
Everything
```

---

Modern Linux

```text
Capability
 │
 ▼
Specific Privilege
```

---

Visualization

```text
Root Privileges
       │
       ▼
+--------------------+
| CAP_CHOWN          |
| CAP_NET_ADMIN      |
| CAP_NET_RAW        |
| CAP_SYS_ADMIN      |
| CAP_SYS_TIME       |
+--------------------+
```

---

Capability Check

```text
Application
      │
      ▼
Kernel Request
      │
      ▼
Capability Present?
      │
   ┌──┴──┐
   │     │
  No    Yes
   │     │
   ▼     ▼
Deny   Allow
```

---

# 13. SELinux Architecture

Traditional Linux

```text
Permissions
      │
      ▼
Access
```

---

SELinux

```text
Permissions
      │
      ▼
SELinux Policy
      │
      ▼
Access
```

---

Type Enforcement

```text
httpd_t
    │
    ▼
shadow_t
    │
    ▼
DENIED
```

---

SELinux Decision Flow

```text
Application
      │
      ▼
Policy Engine
      │
      ▼
Allow / Deny
```

---

# 14. AppArmor Architecture

SELinux

```text
Label Based
```

---

AppArmor

```text
Path Based
```

---

Visualization

```text
Application
      │
      ▼
Path
      │
      ▼
Profile
      │
      ▼
Decision
```

---

Example

```text
nginx
   │
   ▼
/etc/shadow
   │
   ▼
DENIED
```

---

# 15. Container Permission Model

Container

```text
UID

GID

Permissions

Capabilities

SELinux/AppArmor
```

---

Visualization

```text
Container
      │
      ▼
UID/GID
      │
      ▼
Volume Ownership
      │
      ▼
Permissions
      │
      ▼
SELinux
      │
      ▼
Access
```

---

# Docker UID Problem

```text
Container UID
      │
      ▼
1000
```

```text
Host File
      │
      ▼
2000
```

Result

```text
Permission Denied
```

---

# 16. Troubleshooting Map

```text
Permission Denied
        │
        ▼
File Exists?
        │
        ▼
Ownership Correct?
        │
        ▼
Permissions Correct?
        │
        ▼
Directory Traversal OK?
        │
        ▼
ACL Exists?
        │
        ▼
Capability Required?
        │
        ▼
SELinux?
        │
        ▼
AppArmor?
        │
        ▼
Container?
        │
        ▼
Root Cause Found
```

---

# 17. Enterprise Security Model

```text
Users
  │
  ▼
Groups
  │
  ▼
Ownership
  │
  ▼
Permissions
  │
  ▼
ACL
  │
  ▼
Capabilities
  │
  ▼
SELinux/AppArmor
  │
  ▼
Monitoring
```

---

# 18. Linux Permission Mind Map

```text
Linux Permissions
│
├── Ownership
│   ├── User
│   └── Group
│
├── Permissions
│   ├── Read
│   ├── Write
│   └── Execute
│
├── Special Permissions
│   ├── SUID
│   ├── SGID
│   └── Sticky Bit
│
├── ACL
│
├── Capabilities
│
├── SELinux
│
├── AppArmor
│
└── Troubleshooting
```

---

# 19. One-Page Interview Cheat Sheet

```text
Permission Numbers

7 = rwx
6 = rw-
5 = r-x
4 = r--
0 = ---
```

```text
Special Bits

4000 = SUID
2000 = SGID
1000 = Sticky
```

```text
Common Permissions

755 = Public Script
644 = Public File
600 = Secret File
700 = Private Script
```

```text
Key Commands

chmod
chown
chgrp
umask
getfacl
setfacl
getcap
setcap
ls -Z
aa-status
```

---

# 20. Linux Security Pyramid

```text
                 Monitoring
                      ▲
                      │
             SELinux/AppArmor
                      ▲
                      │
                Capabilities
                      ▲
                      │
                     ACL
                      ▲
                      │
         Traditional Permissions
                      ▲
                      │
                  Ownership
                      ▲
                      │
                 Filesystem
```

---

# Key Takeaways

1. Linux security is layered.

2. Ownership is evaluated before permissions.

3. ACL extends traditional permissions.

4. SUID changes effective user identity.

5. SGID controls group inheritance.

6. Sticky Bit protects shared directories.

7. Capabilities replace many root privileges.

8. SELinux uses labels and policies.

9. AppArmor uses paths and profiles.

10. Troubleshooting follows a predictable decision tree.
