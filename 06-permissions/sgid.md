# SGID (Set Group ID)

> SGID (Set Group ID) is a special Linux permission that allows files and directories to inherit or execute with group privileges.

SGID solves one of the biggest problems in Linux:

```text
How do multiple users
work on the same files
without constantly fixing permissions?
```

Without SGID:

```text
Team collaboration becomes difficult.
```

With SGID:

```text
Group ownership stays consistent.
```

---

# Learning Objectives

After reading this guide you will understand:

✅ What SGID is

✅ Why Linux needs SGID

✅ Real GID vs Effective GID

✅ SGID on files

✅ SGID on directories

✅ Group inheritance

✅ Team collaboration environments

✅ Internal kernel behavior

✅ Enterprise use cases

✅ Security implications

✅ Troubleshooting SGID

---

# Why SGID Exists

Imagine a software team.

Members:

```text
alice
bob
charlie
```

All belong to:

```text
developers
```

Directory:

```text
/project
```

---

# Problem Without SGID

Alice creates:

```text
app.js
```

Ownership:

```text
alice developers
```

Good.

---

Bob creates:

```text
api.js
```

Ownership:

```text
bob developers
```

Still okay.

---

Now imagine:

```text
alice
```

has primary group:

```text
frontend
```

and

```text
bob
```

has primary group:

```text
backend
```

---

Files become:

```text
app.js
    frontend

api.js
    backend
```

Team collaboration breaks.

---

# SGID Solution

Directory:

```text
project/
```

Group:

```text
developers
```

Enable SGID:

```bash
chmod g+s project
```

Now:

```text
All new files inherit
developers group ownership.
```

---

# Collaboration Visualization

Without SGID:

```text
project/

├── app.js
│    frontend

├── api.js
│    backend

└── db.js
     database
```

Mess.

---

With SGID:

```text
project/

├── app.js
│    developers

├── api.js
│    developers

└── db.js
     developers
```

Consistent.

---

# Linux Execution Model

Every process has:

```text
Real GID (RGID)

Effective GID (EGID)

Saved GID
```

---

# Real GID

Represents:

```text
Original group
of process owner
```

---

# Effective GID

Represents:

```text
Permissions currently used
```

during execution.

---

# Normal Process

```text
RGID = developers

EGID = developers
```

---

# SGID Process

```text
RGID = developers

EGID = database-admins
```

Possible.

---

# Visual Example

Normal:

```text
User
 │
 ▼
Program
 │
 ▼
RGID = users

EGID = users
```

---

SGID:

```text
User
 │
 ▼
Program
 │
 ▼
RGID = users

EGID = admins
```

---

# SGID on Files

Less common today.

Purpose:

```text
Run program
with file's group privileges.
```

---

# Example

File:

```text
database-tool
```

Owner:

```text
root
```

Group:

```text
dbadmins
```

SGID enabled:

```text
EGID becomes dbadmins
```

during execution.

---

# Permission Display

Normal:

```bash
-rwxr-xr-x
```

SGID:

```bash
-rwxr-sr-x
```

Notice:

```text
s replaces group execute bit.
```

---

# Visual Breakdown

```text
-rwxr-sr-x

     │││
     ││└─ Execute
     │└── SGID
     └── Group
```

---

# Setting SGID

Symbolic:

```bash
chmod g+s file
```

---

Numeric:

```bash
chmod 2755 file
```

---

Why 2?

Special Bits:

```text
4000 → SUID

2000 → SGID

1000 → Sticky Bit
```

---

# Removing SGID

```bash
chmod g-s file
```

or

```bash
chmod 755 file
```

---

# SGID on Directories

This is where SGID shines.

Most real-world usage:

```text
Shared directories
```

---

# Internal Kernel Behavior

Without SGID:

When file created:

```text
New File Group
=
User Primary Group
```

---

Example:

User:

```text
alice
```

Primary Group:

```text
frontend
```

Creates:

```text
app.js
```

Result:

```text
frontend
```

---

# With SGID

Kernel changes behavior.

When file created:

```text
New File Group
=
Parent Directory Group
```

---

Example

Directory:

```text
project
```

Group:

```text
developers
```

SGID enabled.

---

Alice creates:

```text
app.js
```

Result:

```text
developers
```

---

Bob creates:

```text
api.js
```

Result:

```text
developers
```

---

# Kernel Decision Flow

Without SGID:

```text
Create File
      │
      ▼
Use User Primary Group
      │
      ▼
Assign Ownership
```

---

With SGID:

```text
Create File
      │
      ▼
Directory SGID?
      │
   YES
      │
      ▼
Use Directory Group
      │
      ▼
Assign Ownership
```

---

# Verify SGID

Use:

```bash
ls -ld project
```

Output:

```text
drwxr-sr-x
```

Notice:

```text
s
```

in group section.

---

# Real Enterprise Example

Company:

```text
Backend Team
```

Directory:

```text
/company/backend
```

Group:

```text
backend
```

Permissions:

```bash
chgrp backend /company/backend

chmod 2775 /company/backend
```

---

Result:

```text
Every file automatically
belongs to backend group.
```

---

# Git Repository Example

Shared repository:

```text
/project/repo
```

Team:

```text
developers
```

Configuration:

```bash
chgrp developers repo

chmod 2775 repo
```

---

New files:

```text
Always inherit developers group.
```

Perfect collaboration.

---

# SGID and Umask

SGID controls:

```text
Group ownership
```

NOT:

```text
Permissions
```

Permissions still depend on:

```text
umask
```

---

Example

Directory:

```bash
chmod 2775 project
```

User umask:

```bash
002
```

New file:

```text
664
developers
```

---

# SGID in Docker

Container:

```text
UID 1000

GID 1000
```

Volume:

```text
developers
```

SGID ensures:

```text
Consistent group ownership.
```

---

# Kubernetes Example

Shared Persistent Volume:

```text
pv-data
```

Group:

```text
2000
```

SGID:

```text
Enabled
```

All pods:

```text
Create files with same group.
```

Useful for:

```text
Shared storage
```

---

# Security Benefits

SGID prevents:

```text
Permission chaos
```

in collaborative environments.

---

Benefits:

```text
Consistent group ownership

Simplified administration

Predictable access control
```

---

# Security Risks

If group permissions are too broad:

```text
Sensitive files become accessible.
```

---

Bad Example:

```text
finance group
```

contains:

```text
Too many users
```

SGID propagates access.

---

# Auditing SGID Files

Find SGID files:

```bash
find / -perm -2000 -type f 2>/dev/null
```

---

Find SGID directories:

```bash
find / -perm -2000 -type d 2>/dev/null
```

---

# Common SGID Programs

Historically:

```text
wall

write

mail

locate
```

Modern systems use SGID less frequently.

---

# Troubleshooting

---

# New Files Not Inheriting Group

Check:

```bash
ls -ld project
```

Verify:

```text
SGID bit exists
```

---

# SGID Not Showing

Expected:

```text
drwxr-sr-x
```

Not:

```text
drwxr-xr-x
```

---

# Group Ownership Wrong

Check:

```bash
stat file
```

Verify:

```text
Parent directory group
```

---

# Files Have Correct Group But Wrong Permissions

Likely:

```text
umask issue
```

Not SGID.

---

# SGID vs SUID

| Feature | SUID | SGID |
|----------|----------|----------|
| Affects User ID | Yes | No |
| Affects Group ID | No | Yes |
| Common On Files | Yes | Rare |
| Common On Directories | No | Very Common |
| Privilege Elevation | User | Group |
| Collaboration Use | No | Yes |

---

# Interview Questions

---

## What is SGID?

Allows execution or inheritance using group privileges.

---

## What does SGID do on directories?

Forces new files to inherit directory group ownership.

---

## What does SGID do on files?

Runs process with file's group privileges.

---

## How do you set SGID?

```bash
chmod g+s file
```

or

```bash
chmod 2755 file
```

---

## How do you find SGID files?

```bash
find / -perm -2000
```

---

## Why is SGID important?

Maintains consistent group ownership in shared environments.

---

# Quick Cheat Sheet

```bash
chmod g+s project

chmod 2775 project

chmod g-s project

find / -perm -2000

find / -perm -2000 -type d

ls -ld project

stat file
```

---

# Visual Summary

Without SGID:

```text
Alice
  │
  ▼
app.js
  │
  ▼
frontend
```

```text
Bob
  │
  ▼
api.js
  │
  ▼
backend
```

---

With SGID:

```text
Alice
  │
  ▼
app.js
  │
  ▼
developers
```

```text
Bob
  │
  ▼
api.js
  │
  ▼
developers
```

---

Kernel Logic:

```text
Create File
      │
      ▼
Directory SGID?
      │
  YES
      │
      ▼
Inherit Directory Group
      │
      ▼
Assign Ownership
```

---

# Key Takeaways

1. SGID affects group privileges.

2. SGID on files changes Effective GID.

3. SGID on directories enforces group inheritance.

4. SGID is heavily used in shared projects.

5. SGID solves collaboration problems.

6. SGID does not control permissions.

7. umask still controls file permissions.

8. SGID is essential for enterprise team environments.

9. Docker and Kubernetes often rely on SGID behavior.

10. Understanding SGID is fundamental for Linux administration.

---

# Next Step

Continue to:

```text
sticky-bit.md
```

where you'll learn how Linux protects shared directories like `/tmp`, why users cannot delete each other's files there, how the Sticky Bit works internally, and why it is critical for multi-user system security.
