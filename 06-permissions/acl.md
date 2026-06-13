# Access Control Lists (ACL)

> ACL (Access Control List) extends the traditional Linux permission model by allowing permissions to be assigned to specific users and groups beyond the standard Owner–Group–Others model.

ACLs solve one of the biggest limitations of traditional Linux permissions:

```text
What if one specific user
needs access to a file
without becoming owner
or changing the group?
```

Traditional permissions cannot solve this elegantly.

ACLs can.

---

# Learning Objectives

After reading this guide you will understand:

✅ Why ACLs exist

✅ Limitations of traditional permissions

✅ Access ACLs

✅ Default ACLs

✅ ACL Masks

✅ Effective Permissions

✅ Internal ACL Storage

✅ ACL Evaluation Order

✅ Enterprise Collaboration

✅ NFS/Samba ACLs

✅ Security Auditing

✅ ACL Troubleshooting

---

# The Traditional Permission Problem

Suppose:

```bash
-rw-r----- alice developers payroll.xlsx
```

Meaning:

```text
Owner:
alice

Group:
developers

Others:
none
```

---

# New Requirement

Need:

```text
Alice     → Read + Write
Developers→ Read
Bob       → Read
Charlie   → Read + Write
```

Traditional permissions:

```text
Cannot do this.
```

---

# Why Not?

Traditional Linux supports:

```text
1 Owner

1 Group

Everyone Else
```

Only three permission categories.

---

# Visualization

Traditional Model:

```text
                File
                  │
                  ▼
      ┌─────────────────────┐
      │ Owner               │
      ├─────────────────────┤
      │ Group               │
      ├─────────────────────┤
      │ Others              │
      └─────────────────────┘
```

Only 3 permission buckets.

---

# ACL Solution

ACL introduces:

```text
Named Users
Named Groups
Default Permissions
Fine-Grained Rules
```

---

# ACL Visualization

```text
payroll.xlsx

Owner:
alice

Group:
developers

ACL:
bob      → r--
charlie  → rw-
auditors → r--
```

Much more flexible.

---

# ACL Architecture

Traditional permissions remain.

ACL adds additional entries.

```text
User Request
      │
      ▼
Owner Check
      │
      ▼
ACL Check
      │
      ▼
Group Check
      │
      ▼
Others Check
```

---

# Types of ACLs

Linux supports:

```text
1. Access ACL
2. Default ACL
```

---

# Access ACL

Controls:

```text
Existing file permissions
```

Example:

```text
Allow Bob read access
```

---

# Default ACL

Controls:

```text
Permissions inherited
by newly created files
```

inside a directory.

---

# ACL Terminology

ACL contains entries.

Common entries:

```text
user::

group::

other::

mask::

user:bob:

group:finance:
```

---

# Viewing ACLs

Command:

```bash
getfacl file.txt
```

Example:

```text
# file: file.txt

user::rw-
user:bob:r--
group::r--
mask::r--
other::---
```

---

# Understanding ACL Output

---

## Owner Entry

```text
user::rw-
```

Owner permissions.

---

## Named User

```text
user:bob:r--
```

Bob gets read access.

---

## Group Entry

```text
group::r--
```

Group permissions.

---

## Mask Entry

```text
mask::r--
```

Maximum permissions ACL entries may use.

---

## Others Entry

```text
other::---
```

Everyone else.

---

# ACL Internal Storage

ACLs are not stored in:

```text
Permission Bits
```

Instead Linux stores ACLs as:

```text
Extended Attributes (xattr)
```

attached to inode metadata.

---

# Internal Storage Visualization

```text
File
 │
 ▼
Inode
 │
 ├── Owner
 ├── Group
 ├── Permission Bits
 ├── Timestamps
 └── ACL Metadata
```

---

# Setting ACLs

Command:

```bash
setfacl
```

---

# Add User ACL

Example:

```bash
setfacl -m u:bob:r file.txt
```

Meaning:

```text
Bob can read file.txt
```

---

# Verify

```bash
getfacl file.txt
```

Output:

```text
user:bob:r--
```

---

# Add Read + Write

```bash
setfacl -m u:bob:rw file.txt
```

---

# Add Group ACL

```bash
setfacl -m g:finance:r file.txt
```

---

# Multiple ACL Entries

```bash
setfacl -m u:bob:r,u:charlie:rw file.txt
```

---

# ACL Visualization

```text
report.txt

Owner:
alice

ACL:
bob      → r--
charlie  → rw-
finance  → r--
```

---

# Removing ACL Entries

Remove Bob:

```bash
setfacl -x u:bob file.txt
```

---

# Remove Group Entry

```bash
setfacl -x g:finance file.txt
```

---

# Remove All ACLs

```bash
setfacl -b file.txt
```

Back to traditional permissions.

---

# ACL Mask

One of the most misunderstood concepts.

---

# What Is Mask?

ACL Mask defines:

```text
Maximum permissions
allowed for ACL entries.
```

---

# Example

ACL:

```text
user:bob:rwx
```

Mask:

```text
mask::r--
```

Result:

```text
Bob effectively gets:

r--
```

NOT:

```text
rwx
```

---

# Visualization

```text
Requested:
rwx

Mask:
r--

Result:
r--
```

---

# Effective Permission Concept

Linux calculates:

```text
ACL Entry
      │
      ▼
Apply Mask
      │
      ▼
Effective Permission
```

---

# View Effective Permissions

```bash
getfacl file.txt
```

May show:

```text
user:bob:rwx #effective:r--
```

Very important for troubleshooting.

---

# Default ACLs

Default ACLs affect:

```text
New files
New directories
```

inside a directory.

---

# Example

Directory:

```bash
project/
```

Set:

```bash
setfacl -d -m u:bob:rwx project
```

---

# Creation Flow

```text
project/
      │
      ▼
Default ACL Exists?
      │
     YES
      │
      ▼
Apply ACL
      │
      ▼
Create New File
```

---

# Verify Default ACL

```bash
getfacl project
```

Output:

```text
default:user:bob:rwx
```

---

# Enterprise Example

Development Team:

```text
Alice
Bob
Charlie
QA Team
DevOps Team
```

Directory:

```text
/project
```

ACLs:

```text
Bob      → rwx
Charlie  → rwx
QA       → r--
DevOps   → rwx
```

No ownership changes required.

---

# ACL Evaluation Order

Linux evaluates access carefully.

---

# Simplified Flow

```text
Access Request
       │
       ▼
Owner?
       │
      YES
       │
       ▼
Owner Permissions
```

---

If not owner:

```text
Named ACL?
      │
     YES
      │
      ▼
Apply ACL + Mask
```

---

If not:

```text
Group ACL
      │
      ▼
Apply ACL + Mask
```

---

Finally:

```text
Others
```

---

# ACL vs chmod

Many admins get confused.

---

# chmod Changes ACL Mask

Example:

```bash
chmod g-w file.txt
```

May modify:

```text
ACL Mask
```

not individual ACL entries.

---

# Visualization

```text
ACL Entry:
bob:rwx

chmod g-w

Mask becomes:
r-x

Bob now loses write access
```

---

# ACL Indicator in ls

View:

```bash
ls -l
```

Example:

```text
-rw-r-----+
```

Notice:

```text
+
```

Means:

```text
ACL Exists
```

---

# Security Benefits

ACLs provide:

```text
Fine-Grained Access

Reduced Ownership Changes

Better Collaboration

Least Privilege
```

---

# Security Risks

ACLs can become:

```text
Complex

Hidden

Hard to Audit
```

Administrators may miss:

```text
Unexpected Access
```

---

# ACL Auditing

Find ACL-enabled files:

```bash
getfacl -R directory
```

---

# Search for ACLs

```bash
find / -exec getfacl {} \; 2>/dev/null
```

---

# ACL in NFS

Modern NFS supports ACLs.

Useful for:

```text
Shared Storage

Enterprise File Servers
```

---

# ACL in Samba

Windows permissions often map to:

```text
Linux ACLs
```

behind the scenes.

---

# ACL in Containers

Containers may use ACLs on:

```text
Persistent Volumes
Shared Data
```

for fine-grained access.

---

# Common Mistakes

---

## Forgetting the Mask

Most common ACL issue.

ACL appears correct.

Access still denied.

Mask blocks it.

---

## Ignoring the "+" Symbol

```bash
ls -l
```

Shows:

```text
+
```

ACL exists.

---

## Mixing chmod and ACLs

chmod may change ACL behavior.

---

## Excessive ACL Complexity

Too many ACL entries create management problems.

---

# Troubleshooting ACLs

---

# User Cannot Access File

Check:

```bash
getfacl file.txt
```

---

# ACL Looks Correct

Check:

```text
mask::
```

entry.

---

# Permissions Unexpected

Verify:

```bash
chmod
```

did not alter ACL mask.

---

# ACL Not Working

Filesystem may not support ACLs.

Check mount options.

---

# Interview Questions

---

## What problem do ACLs solve?

Fine-grained permissions beyond Owner–Group–Others.

---

## What command displays ACLs?

```bash
getfacl
```

---

## What command modifies ACLs?

```bash
setfacl
```

---

## What does "+" mean in ls output?

ACL exists.

---

## What is an ACL Mask?

Maximum permissions allowed for ACL entries.

---

## Difference between Access ACL and Default ACL?

Access ACL:

```text
Current file permissions
```

Default ACL:

```text
Inherited permissions
```

---

# Quick Cheat Sheet

```bash
getfacl file.txt

setfacl -m u:bob:r file.txt

setfacl -m u:bob:rw file.txt

setfacl -m g:finance:r file.txt

setfacl -x u:bob file.txt

setfacl -b file.txt

setfacl -d -m u:bob:rwx project
```

---

# Visual Summary

Traditional Model:

```text
Owner
Group
Others
```

ACL Model:

```text
Owner
Group
Others

Bob
Charlie
Finance
QA
DevOps
```

---

Permission Flow:

```text
Request
   │
   ▼
Owner?
   │
   ▼
Named ACL?
   │
   ▼
Mask Applied
   │
   ▼
Effective Permission
   │
   ▼
ALLOW / DENY
```

---

# Key Takeaways

1. ACLs extend traditional Linux permissions.

2. ACLs allow permissions for specific users and groups.

3. ACLs are stored as extended attributes.

4. Access ACLs affect current permissions.

5. Default ACLs affect inherited permissions.

6. ACL Masks can silently reduce permissions.

7. `getfacl` and `setfacl` are essential tools.

8. ACLs are heavily used in enterprise environments.

9. Samba and NFS often rely on ACLs.

10. Understanding ACL evaluation is critical for troubleshooting.

---

# Next Step

Continue to:

```text
capabilities.md
```

where you'll learn how modern Linux replaces many dangerous SUID-root programs with **Capabilities**, a kernel mechanism that breaks root privileges into dozens of fine-grained permissions such as `CAP_NET_BIND_SERVICE`, `CAP_SYS_ADMIN`, and `CAP_NET_RAW`.
