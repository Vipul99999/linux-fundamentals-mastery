# Linux Permission Flowcharts

> Linux permissions are easier to understand when visualized as decision trees and system flows rather than individual commands.

This guide contains visual flowcharts for:

```text
Permission Evaluation

Ownership Resolution

Directory Traversal

ACL Evaluation

SUID/SGID

Sticky Bit

Capabilities

SELinux

AppArmor

Permission Troubleshooting
```

Use this guide as a:

```text
Quick Reference

Interview Revision Sheet

Troubleshooting Handbook
```

---

# Linux Security Stack

High-Level View

```text
Application
      │
      ▼
Ownership
      │
      ▼
Traditional Permissions
      │
      ▼
ACL
      │
      ▼
Capabilities
      │
      ▼
SELinux
      │
      ▼
AppArmor
      │
      ▼
Filesystem Rules
      │
      ▼
ALLOW / DENY
```

---

# Complete Access Decision Flow

When a process requests access:

```text
Access Request
       │
       ▼
File Exists?
       │
 ┌─────┴─────┐
 │           │
No          Yes
 │           │
 ▼           ▼
ERROR   Ownership Check
               │
               ▼
         Permission Check
               │
               ▼
            ACL Check
               │
               ▼
       Capability Check
               │
               ▼
        SELinux Check
               │
               ▼
       AppArmor Check
               │
               ▼
         Mount Rules
               │
               ▼
          ALLOW/DENY
```

---

# Ownership Resolution Flow

Linux first determines:

```text
Who Are You?
```

---

Flow

```text
User Accesses File
         │
         ▼
Are You Owner?
         │
   ┌─────┴─────┐
   │           │
 YES          NO
   │           │
   ▼           ▼
Owner Bits   Group Check
                 │
                 ▼
          Group Member?
                 │
           ┌─────┴─────┐
           │           │
          YES         NO
           │           │
           ▼           ▼
      Group Bits    Others Bits
```

---

# Permission Evaluation Flow

Example:

```bash
-rwxr-x---
```

---

Evaluation

```text
Request
   │
   ▼
Owner?
   │
 ┌─┴─┐
 │   │
Y    N
│    │
▼    ▼
Use  Group?
Owner  │
Bits   ▼
      Yes
       │
       ▼
   Use Group Bits
       │
       ▼
      Else
       │
       ▼
   Use Others Bits
```

---

# File Permission Meaning

```text
Read
 │
 ▼
Open File
```

---

```text
Write
 │
 ▼
Modify File
```

---

```text
Execute
 │
 ▼
Run File
```

---

# Directory Permission Meaning

Read:

```text
List Files
```

Visualization:

```text
Directory
    │
    ▼
View Names
```

---

Write:

```text
Create/Delete Entries
```

Visualization:

```text
Directory
     │
     ▼
Add Remove Files
```

---

Execute:

```text
Traverse Directory
```

Visualization:

```text
Directory
      │
      ▼
Enter Directory
```

---

# Directory Traversal Flow

Access:

```text
/home/alice/docs/report.txt
```

Requires:

```text
/
home
alice
docs
```

all executable.

---

Flow

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

If any directory lacks:

```text
Execute Permission
```

Access fails.

---

# chmod Decision Flow

```text
Need Access Change
        │
        ▼
Who Needs Access?
        │
 ┌──────┼──────┐
 │      │      │
Owner Group Others
 │      │      │
 ▼      ▼      ▼
u      g      o
```

---

Examples

```bash
chmod u+r file
```

```bash
chmod g+w file
```

```bash
chmod o-x file
```

---

# Numeric Permission Flow

```text
Permission
     │
     ▼
Read    = 4

Write   = 2

Execute = 1
```

---

Example

```text
7
```

Calculation

```text
4+2+1
```

Result

```text
rwx
```

---

Visualization

```text
7 → rwx

6 → rw-

5 → r-x

4 → r--

0 → ---
```

---

# ACL Evaluation Flow

Linux ACL Evaluation

```text
Request
   │
   ▼
Owner?
   │
 ┌─┴─┐
 │   │
Y    N
│    │
▼    ▼
Owner ACL?
       │
       ▼
Named User ACL?
       │
       ▼
Apply ACL Mask
       │
       ▼
Group ACL?
       │
       ▼
Others
```

---

# ACL Mask Flow

```text
ACL Entry

rwx
 │
 ▼

Mask

r--
 │
 ▼

Effective

r--
```

---

Visualization

```text
Requested
   rwx

Mask
   r--

Result
   r--
```

---

# ACL Structure Diagram

```text
File
 │
 ▼

Owner ACL

Named Users

Group ACL

Named Groups

Mask

Others
```

---

# File Creation Flow

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

# umask Calculation

Files:

```text
666
```

Base.

---

Directories:

```text
777
```

Base.

---

Example

```bash
umask 022
```

Files:

```text
666 - 022

644
```

Directories:

```text
777 - 022

755
```

---

Visualization

```text
Default
 666
  │
  ▼
umask
 022
  │
  ▼
Final
 644
```

---

# SUID Execution Flow

```text
User
 │
 ▼
Execute Program
 │
 ▼
SUID Bit?
 │
 ┌──┴──┐
 │     │
No    Yes
 │     │
 ▼     ▼
Normal EUID
      │
      ▼
File Owner EUID
```

---

Visualization

```text
Alice
  │
  ▼
passwd
  │
  ▼
EUID=root
```

---

# SGID Directory Flow

```text
Create File
      │
      ▼
Directory SGID?
      │
 ┌────┴────┐
 │         │
No        Yes
 │         │
 ▼         ▼
User     Directory
Group    Group
```

---

Visualization

Without SGID

```text
Alice → frontend
Bob   → backend
```

---

With SGID

```text
Alice → developers
Bob   → developers
```

---

# Sticky Bit Flow

Delete Request

```text
User
 │
 ▼
Delete File
 │
 ▼
Sticky Bit?
 │
 ┌──┴──┐
 │     │
No    Yes
 │     │
 ▼     ▼
Allow Ownership Check
```

---

Ownership Check

```text
Own File?
    │
 ┌──┴──┐
 │     │
Yes   No
 │     │
 ▼     ▼
Allow Root?
          │
       ┌──┴──┐
       │     │
      Yes   No
       │     │
       ▼     ▼
    Allow Deny
```

---

# Capability Flow

```text
Application
      │
      ▼
Kernel Request
      │
      ▼
Required Capability?
      │
      ▼
Capability Present?
      │
 ┌────┴────┐
 │         │
No        Yes
 │         │
 ▼         ▼
Deny     Allow
```

---

Example

```text
Port 80
   │
   ▼
CAP_NET_BIND_SERVICE
```

---

# SELinux Decision Flow

```text
Application
      │
      ▼
Permission Check
      │
      ▼
SELinux Context
      │
      ▼
Policy Match?
      │
 ┌────┴────┐
 │         │
No        Yes
 │         │
 ▼         ▼
Deny     Allow
```

---

Visualization

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

# SELinux Architecture Diagram

```text
Application
      │
      ▼
System Call
      │
      ▼
Kernel
      │
      ▼
SELinux Policy
      │
      ▼
Decision
```

---

# AppArmor Decision Flow

```text
Application
      │
      ▼
Access Path
      │
      ▼
Profile Match?
      │
 ┌────┴────┐
 │         │
No        Yes
 │         │
 ▼         ▼
Deny     Allow
```

---

Visualization

```text
nginx
   │
   ▼
/etc/shadow
   │
   ▼
Profile Deny
```

---

# Container Permission Flow

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
SELinux/AppArmor
      │
      ▼
Allow/Deny
```

---

# Docker Volume Flow

```text
Container UID
      │
      ▼
Host Files
      │
      ▼
Ownership Match?
      │
 ┌────┴────┐
 │         │
No        Yes
 │         │
 ▼         ▼
Deny     Continue
```

---

# Troubleshooting Master Flowchart

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
Group Membership OK?
        │
        ▼
ACL Exists?
        │
        ▼
Mount Restrictions?
        │
        ▼
Capabilities Required?
        │
        ▼
SELinux Blocking?
        │
        ▼
AppArmor Blocking?
        │
        ▼
Container Issue?
        │
        ▼
Root Cause Found
```

---

# Enterprise Permission Architecture

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
Filesystem
  │
  ▼
Applications
```

---

# Linux Security Layers Visual

```text
+--------------------------------+
| Monitoring                     |
+--------------------------------+
| SELinux / AppArmor             |
+--------------------------------+
| Capabilities                   |
+--------------------------------+
| ACL                            |
+--------------------------------+
| Traditional Permissions        |
+--------------------------------+
| Ownership                      |
+--------------------------------+
| Filesystem                     |
+--------------------------------+
```

---

# Interview Quick Reference

Permission Order

```text
Owner
Group
Others
```

---

ACL Order

```text
Owner

Named User

Group

Named Group

Mask

Others
```

---

Security Layers

```text
Permissions

ACL

Capabilities

SELinux

AppArmor
```

---

# Key Takeaways

1. Linux access decisions follow a predictable flow.

2. Ownership is evaluated before permissions.

3. Directory execute permission is essential.

4. ACL masks frequently affect results.

5. SUID changes effective user identity.

6. SGID controls group inheritance.

7. Sticky Bit protects shared directories.

8. Capabilities replace many root privileges.

9. SELinux and AppArmor provide additional enforcement.

10. Troubleshooting becomes easier when visualized as a decision tree.

---

# Next Step

Continue to:

```text
labs.md
```

This file should contain 50+ hands-on labs ranging from beginner to advanced:

- Permission labs
- chmod labs
- ACL labs
- SUID labs
- SGID labs
- Sticky Bit labs
- Capability labs
- SELinux labs
- AppArmor labs
- Docker permission labs
- Real-world troubleshooting labs

so learners can practice every concept from this section.
