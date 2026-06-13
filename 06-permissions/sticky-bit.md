# Sticky Bit

> The Sticky Bit is a special Linux permission that restricts file deletion and renaming inside shared directories.

Without Sticky Bit:

```text
Any user with write access
to a directory can delete
any file inside that directory.
```

With Sticky Bit:

```text
Users can only delete:

• Their own files
• Files owned by directory owner
• Files deleted by root
```

Sticky Bit is one of Linux's most important protections for multi-user systems.

---

# Learning Objectives

After reading this guide you will understand:

✅ What Sticky Bit is

✅ Why Linux needs Sticky Bit

✅ File deletion internals

✅ Directory write permissions

✅ Sticky Bit kernel behavior

✅ /tmp security

✅ Multi-user environments

✅ Shared directory protection

✅ Enterprise use cases

✅ Security best practices

✅ Troubleshooting Sticky Bit

---

# Why Sticky Bit Exists

Imagine a shared directory:

```text
/shared
```

Users:

```text
alice
bob
charlie
```

Permissions:

```text
drwxrwxrwx
```

Everyone can:

```text
Read
Write
Enter
```

Looks useful.

---

# The Problem

Alice creates:

```text
report.txt
```

Bob can run:

```bash
rm report.txt
```

File deleted.

---

# Why?

Because Linux deletion is controlled by:

```text
Directory Permissions
```

NOT

```text
File Permissions
```

This surprises many Linux users.

---

# Important Concept

Suppose:

```bash
-r-------- report.txt
```

No write permission.

Yet:

```bash
rm report.txt
```

may still work.

Why?

Because deletion affects:

```text
Directory Entries
```

not file contents.

---

# Internal Linux View

Directory:

```text
/shared
```

Contains:

```text
report.txt
notes.txt
data.csv
```

Internally:

```text
Directory
   │
   ├── report.txt -> inode 123
   ├── notes.txt  -> inode 456
   └── data.csv   -> inode 789
```

Deleting a file removes:

```text
Filename → inode link
```

from the directory.

---

# Deletion Flow

```text
User
  │
  ▼
rm file.txt
  │
  ▼
Kernel checks
Directory permissions
  │
  ▼
Remove directory entry
```

Notice:

```text
File permissions
are not checked first.
```

---

# Shared Directory Problem

Example:

```text
/shared
```

Permissions:

```text
777
```

Users:

```text
Alice
Bob
Charlie
```

---

Alice creates:

```text
salary.xlsx
```

Bob deletes:

```bash
rm salary.xlsx
```

Allowed.

---

Charlie renames:

```bash
mv salary.xlsx hacked.xlsx
```

Allowed.

---

This creates chaos.

---

# Sticky Bit Solution

Linux introduces:

```text
Sticky Bit
```

When enabled:

```text
Users cannot delete
other users' files.
```

---

# Sticky Bit Rule

Deletion allowed if:

```text
1. User owns file

OR

2. User owns directory

OR

3. User is root
```

Otherwise:

```text
Permission Denied
```

---

# Visual Example

Without Sticky Bit:

```text
shared/

├── alice.txt
├── bob.txt
└── charlie.txt
```

Any user:

```text
Can delete everything
```

---

With Sticky Bit:

```text
shared/

├── alice.txt
├── bob.txt
└── charlie.txt
```

Alice:

```text
Can delete alice.txt
```

Cannot delete:

```text
bob.txt
charlie.txt
```

---

# Sticky Bit Representation

Normal:

```bash
drwxrwxrwx
```

Sticky Bit:

```bash
drwxrwxrwt
```

Notice:

```text
t
```

at the end.

---

# Visual Breakdown

```text
drwxrwxrwt

        │
        └── Sticky Bit
```

---

# Setting Sticky Bit

Symbolic:

```bash
chmod +t directory
```

Example:

```bash
chmod +t shared
```

---

# Numeric Method

```bash
chmod 1777 shared
```

---

# Why 1?

Special permission bits:

```text
4000 = SUID

2000 = SGID

1000 = Sticky Bit
```

---

# Verify Sticky Bit

Use:

```bash
ls -ld shared
```

Output:

```text
drwxrwxrwt
```

---

# Removing Sticky Bit

```bash
chmod -t shared
```

or

```bash
chmod 777 shared
```

---

# Kernel Decision Process

User:

```text
bob
```

Attempts:

```bash
rm alice.txt
```

---

Kernel Flow

```text
Delete Request
       │
       ▼
Sticky Bit Enabled?
       │
     YES
       │
       ▼
Own File?
       │
    No
       │
       ▼
Own Directory?
       │
    No
       │
       ▼
Root?
       │
    No
       │
       ▼
DENY
```

---

# The Most Famous Sticky Bit Directory

```text
/tmp
```

---

# What Is /tmp?

Temporary storage.

Used by:

```text
Applications
Users
Services
Installers
Browsers
Databases
```

---

# /tmp Permissions

View:

```bash
ls -ld /tmp
```

Output:

```text
drwxrwxrwt
```

---

Why?

Everyone needs:

```text
Read
Write
Execute
```

But users must not delete each other's files.

---

# /tmp Visualization

```text
/tmp

├── alice.lock
├── bob.cache
├── chrome.tmp
├── mysql.sock
└── install.log
```

Without Sticky Bit:

```text
Anyone could delete everything.
```

---

With Sticky Bit:

```text
Only owners can delete.
```

---

# Sticky Bit on Files

Historically:

```text
Executable files
```

used Sticky Bit.

Modern Linux:

```text
Ignored on regular files.
```

Today:

```text
Sticky Bit is meaningful
primarily on directories.
```

---

# Shared Team Directory Example

Company:

```text
/department/shared
```

Permissions:

```bash
chmod 1777 shared
```

Employees:

```text
Can create files
Cannot delete others' files
```

---

# University Lab Example

Students:

```text
100+
```

Shared workspace:

```text
/lab/work
```

Sticky Bit prevents:

```text
Accidental deletions
```

---

# Container Example

Shared volume:

```text
/shared-volume
```

Multiple containers:

```text
app1
app2
app3
```

Sticky Bit prevents:

```text
Cross-container file deletion
```

when users differ.

---

# Kubernetes Example

Shared Persistent Volume:

```text
NFS Volume
```

Multiple Pods:

```text
frontend
backend
jobs
```

Sticky Bit can provide:

```text
Basic deletion protection
```

---

# Security Benefits

Sticky Bit prevents:

```text
Accidental deletion

Malicious deletion

Shared workspace disruption
```

---

# Security Limitations

Sticky Bit does NOT prevent:

```text
Reading files

Modifying files

Changing permissions
```

if permissions allow it.

---

Example:

```text
Sticky Bit enabled
```

But:

```bash
chmod 777 file.txt
```

Users may still modify contents.

---

# Sticky Bit vs File Permissions

Sticky Bit protects:

```text
Directory entries
```

Permissions protect:

```text
File contents
```

Different concepts.

---

# Sticky Bit vs SGID

| Feature | Sticky Bit | SGID |
|----------|------------|---------|
| Affects Ownership | No | Yes |
| Group Inheritance | No | Yes |
| Deletion Protection | Yes | No |
| Shared Directories | Yes | Yes |
| Collaboration | Partial | Strong |

---

# Sticky Bit vs SUID

| Feature | Sticky Bit | SUID |
|----------|------------|---------|
| Process Privileges | No | Yes |
| File Deletion Control | Yes | No |
| Effective UID Changes | No | Yes |
| Shared Directory Security | Yes | No |

---

# Enterprise Best Practices

---

## Use on Shared Directories

Examples:

```text
/tmp

/var/tmp

/shared
```

---

## Combine With SGID

Example:

```bash
chmod 3775 project
```

Meaning:

```text
SGID + Sticky Bit
```

Provides:

```text
Group inheritance
+
Deletion protection
```

---

# Audit Shared Locations

Check:

```bash
find / -perm -1000 -type d
```

---

# Common Mistakes

---

## Using 777 Without Sticky Bit

Bad:

```bash
chmod 777 shared
```

Anyone can delete anything.

---

## Expecting Sticky Bit to Protect Contents

Wrong.

It protects:

```text
Deletion
Renaming
```

Not editing.

---

## Using Sticky Bit on Files

Mostly useless today.

---

## Forgetting Ownership Rules

Directory owner can still delete files.

---

# Troubleshooting

---

## User Cannot Delete Own File

Check:

```bash
ls -l file
```

Ownership may be incorrect.

---

## User Deletes Other Files

Verify:

```bash
ls -ld directory
```

Expected:

```text
drwxrwxrwt
```

Not:

```text
drwxrwxrwx
```

---

## Sticky Bit Missing

Enable:

```bash
chmod +t directory
```

---

# Auditing Sticky Bit Directories

Find all:

```bash
find / -perm -1000 -type d 2>/dev/null
```

---

# Interview Questions

---

## What is Sticky Bit?

A special directory permission that restricts file deletion and renaming.

---

## How is Sticky Bit shown?

```text
t
```

Example:

```text
drwxrwxrwt
```

---

## What does chmod 1777 mean?

```text
Sticky Bit
+
rwxrwxrwx
```

---

## Why does /tmp use Sticky Bit?

Allows everyone to create files while preventing users from deleting each other's files.

---

## Does Sticky Bit prevent file modification?

No.

Only deletion and renaming protection.

---

## How do you find Sticky Bit directories?

```bash
find / -perm -1000 -type d
```

---

# Quick Cheat Sheet

```bash
chmod +t shared

chmod 1777 shared

chmod -t shared

ls -ld shared

find / -perm -1000 -type d

ls -ld /tmp
```

---

# Visual Summary

Without Sticky Bit:

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

With Sticky Bit:

```text
Alice File
     │
     ▼
Bob Deletes
     │
     ▼
Kernel Check
     │
     ▼
DENIED
```

---

Kernel Logic:

```text
Delete Request
      │
      ▼
Sticky Bit Enabled?
      │
    YES
      │
      ▼
Own File?
      │
      ▼
Own Directory?
      │
      ▼
Root?
      │
      ▼
ALLOW / DENY
```

---

# Key Takeaways

1. Sticky Bit protects shared directories.

2. It controls deletion and renaming.

3. It does not control file contents.

4. `/tmp` is the most famous Sticky Bit directory.

5. Sticky Bit is shown as `t`.

6. `chmod 1777` enables Sticky Bit.

7. Sticky Bit works primarily on directories.

8. It is essential for multi-user Linux systems.

9. Enterprises use it heavily in shared storage areas.

10. Understanding Sticky Bit is fundamental for Linux security.

---

# Next Step

Continue to:

```text
acl.md
```

where you'll learn how Linux goes beyond the traditional Owner–Group–Others model and provides fine-grained permissions for individual users and groups using **Access Control Lists (ACLs)**.
