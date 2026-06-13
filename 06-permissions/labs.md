# Linux Permission Labs

> The fastest way to master Linux permissions is through hands-on practice.

This lab guide contains practical exercises covering:

```text
Ownership

Permissions

chmod

chown

Groups

umask

SUID

SGID

Sticky Bit

ACL

Capabilities

SELinux

AppArmor

Containers

Troubleshooting
```

---

# Lab Structure

Every lab includes:

```text
Objective

Setup

Task

Expected Result

Verification

Challenge Extension
```

---

# Lab 1

Understanding Permission Bits

Difficulty:

```text
Beginner
```

---

# Objective

Understand:

```text
r

w

x
```

---

# Setup

```bash
mkdir lab1

cd lab1

touch file.txt
```

---

# Task

Give:

```text
Owner: Read Write

Group: Read

Others: None
```

---

# Expected Result

```text
-rw-r-----
```

---

# Verification

```bash
ls -l
```

---

# Challenge

Convert to numeric notation.

---

# Lab 2

chmod Symbolic Mode

Difficulty:

```text
Beginner
```

---

# Setup

```bash
touch script.sh
```

---

# Task

Add execute permission for owner.

---

# Expected

```text
-rwx------
```

---

# Verification

```bash
ls -l script.sh
```

---

# Lab 3

chmod Numeric Mode

Difficulty:

```text
Beginner
```

---

# Task

Apply:

```text
755
```

to a script.

---

# Verification

```bash
ls -l
```

Expected:

```text
-rwxr-xr-x
```

---

# Lab 4

Ownership Basics

Difficulty:

```text
Beginner
```

---

# Objective

Understand ownership.

---

# Setup

Create:

```bash
touch report.txt
```

---

# Task

Change owner.

```bash
chown user report.txt
```

---

# Verify

```bash
ls -l
```

---

# Lab 5

Group Ownership

Difficulty:

```text
Beginner
```

---

# Task

Assign file to group:

```text
developers
```

---

# Verify

```bash
ls -l
```

---

# Lab 6

Directory Traversal

Difficulty:

```text
Beginner
```

---

# Setup

```bash
mkdir secure
touch secure/data.txt
```

---

# Task

Remove execute permission from directory.

---

# Observe

Can you access:

```text
data.txt
```

?

---

# Lesson

Directory execute permission matters.

---

# Lab 7

Permission Denied Investigation

Difficulty:

```text
Intermediate
```

---

# Setup

```bash
touch secret.txt

chmod 600 secret.txt
```

---

# Task

Access as another user.

---

# Questions

```text
Why denied?

Which permission failed?
```

---

# Lab 8

Understanding umask

Difficulty:

```text
Intermediate
```

---

# Setup

Check:

```bash
umask
```

---

# Task

Set:

```bash
umask 027
```

Create files.

---

# Observe

Permissions generated.

---

# Verification

```bash
ls -l
```

---

# Lab 9

Compare Different umask Values

Test:

```text
022

027

077
```

---

# Create

```bash
touch testfile
```

---

# Compare Results

Build a table.

---

# Lab 10

SUID Basics

Difficulty:

```text
Intermediate
```

---

# Objective

Understand SUID behavior.

---

# Setup

Create test binary.

---

# Task

Apply:

```bash
chmod 4755 binary
```

---

# Verify

```bash
ls -l
```

Expected:

```text
-rwsr-xr-x
```

---

# Lab 11

Find SUID Programs

---

# Task

Discover all SUID binaries.

```bash
find / -perm -4000 -type f
```

---

# Questions

```text
Why does passwd use SUID?
```

---

# Lab 12

SGID Directory

Difficulty:

```text
Intermediate
```

---

# Setup

```bash
mkdir project
```

---

# Task

Apply:

```bash
chmod 2775 project
```

---

# Create Files

Using multiple users.

---

# Observe

Group inheritance.

---

# Lab 13

Sticky Bit

Difficulty:

```text
Intermediate
```

---

# Setup

```bash
mkdir shared
chmod 1777 shared
```

---

# Task

Create files as different users.

---

# Attempt

Delete another user's file.

---

# Observe

Permission denied.

---

# Lab 14

ACL Basics

Difficulty:

```text
Intermediate
```

---

# Setup

```bash
touch finance.txt
```

---

# Task

Grant Bob read access.

```bash
setfacl -m u:bob:r finance.txt
```

---

# Verify

```bash
getfacl finance.txt
```

---

# Lab 15

ACL Mask

Difficulty:

```text
Intermediate
```

---

# Task

Grant:

```text
rwx
```

Apply restrictive mask.

---

# Observe

Effective permissions.

---

# Lab 16

Default ACLs

Difficulty:

```text
Intermediate
```

---

# Setup

```bash
mkdir team
```

---

# Task

Configure default ACL.

---

# Create Files

Observe inheritance.

---

# Lab 17

Capability Basics

Difficulty:

```text
Advanced
```

---

# Objective

View capabilities.

---

# Task

```bash
getcap -r /
```

---

# Questions

```text
Which binaries use capabilities?
```

---

# Lab 18

Grant Capability

Difficulty:

```text
Advanced
```

---

# Task

Assign:

```text
CAP_NET_BIND_SERVICE
```

to a test binary.

---

# Verify

```bash
getcap binary
```

---

# Lab 19

SELinux Context Exploration

Difficulty:

```text
Advanced
```

---

# Task

View contexts.

```bash
ls -Z
```

---

# Observe

Different file labels.

---

# Lab 20

SELinux Mode Testing

Difficulty:

```text
Advanced
```

---

# Task

Check:

```bash
getenforce
```

---

Switch:

```bash
setenforce 0
```

---

Observe behavior.

---

# Lab 21

Restore SELinux Contexts

---

# Task

Break labels.

Restore:

```bash
restorecon
```

---

# Verify

```bash
ls -Z
```

---

# Lab 22

AppArmor Profile Exploration

Difficulty:

```text
Advanced
```

---

# Task

```bash
aa-status
```

---

Review loaded profiles.

---

# Lab 23

AppArmor Modes

---

# Switch

```bash
aa-complain
```

---

Then:

```bash
aa-enforce
```

---

Observe logs.

---

# Lab 24

World Writable Audit

Difficulty:

```text
Advanced
```

---

# Task

Find:

```bash
find / -perm -002
```

---

# Questions

```text
Why dangerous?
```

---

# Lab 25

Security Audit

Difficulty:

```text
Advanced
```

---

# Goal

Audit:

```text
SUID

SGID

ACL

Capabilities
```

---

# Build Report

Include findings.

---

# Lab 26

Web Server Permission Failure

Difficulty:

```text
Advanced
```

---

# Scenario

Website returns:

```text
403
```

---

# Investigate

```text
Ownership

Permissions

SELinux
```

---

# Find Root Cause

---

# Lab 27

Database Backup Security

Difficulty:

```text
Advanced
```

---

# Goal

Create secure backups.

---

# Requirement

Only DB admins may access.

---

# Implement

```text
Ownership

Permissions

ACL
```

---

# Lab 28

Container UID Mismatch

Difficulty:

```text
Advanced
```

---

# Scenario

Container cannot write mounted volume.

---

# Investigate

```text
UID

GID

Ownership
```

---

# Fix

Without using:

```bash
chmod 777
```

---

# Lab 29

Shared Team Workspace

Difficulty:

```text
Advanced
```

---

# Requirements

```text
Multiple Developers

Shared Files

Deletion Protection
```

---

# Implement

Using:

```text
SGID

Sticky Bit
```

---

# Lab 30

Full Permission Troubleshooting Challenge

Difficulty:

```text
Expert
```

---

# Scenario

Application fails.

Symptoms:

```text
Permission Denied
```

---

Potential Causes

```text
Permissions

Ownership

ACL

SELinux

Capabilities
```

---

# Task

Identify exact cause.

---

# Verification Checklist

For Every Lab

```text
✓ Can Explain Why

✓ Can Explain How

✓ Can Reproduce

✓ Can Verify

✓ Can Troubleshoot
```

---

# Mini Projects

---

# Project 1

Secure Development Team Directory

Requirements:

```text
Group Collaboration

Protected Deletion

Controlled Access
```

Use:

```text
Groups

SGID

Sticky Bit

ACL
```

---

# Project 2

Secure Web Hosting Environment

Requirements:

```text
Web Server

Restricted Files

SELinux
```

---

# Project 3

Enterprise File Server

Requirements:

```text
Departments

ACL

Auditing
```

---

# Project 4

Containerized Application Storage

Requirements:

```text
Docker

Persistent Volumes

UID Mapping
```

---

# Permission Mastery Roadmap

Beginner

```text
Labs 1-6
```

---

Intermediate

```text
Labs 7-16
```

---

Advanced

```text
Labs 17-29
```

---

Expert

```text
Lab 30
```

---

# Visual Learning Path

```text
Ownership
     │
     ▼
Permissions
     │
     ▼
Groups
     │
     ▼
umask
     │
     ▼
ACL
     │
     ▼
SUID/SGID
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
Containers
     │
     ▼
Troubleshooting
```

---

# Key Takeaways

1. Permissions are learned through practice.

2. Every concept should be tested manually.

3. Troubleshooting is more important than memorization.

4. ACLs, SELinux, and containers create most real-world complexity.

5. Security comes from understanding interactions between layers.

6. Hands-on experience is essential for interviews.

7. Labs build intuition.

8. Projects combine multiple concepts.

9. Real-world scenarios improve retention.

10. Mastery comes from repetition and experimentation.

---

# Next Step

Continue to:

```text
visuals.md
```

This file should become the ultimate visual guide containing:

- Permission diagrams
- Ownership maps
- ACL visuals
- SUID/SGID diagrams
- SELinux architecture
- AppArmor architecture
- Container permission diagrams
- Security layer infographics
- Interview cheat sheets
- One-page revision maps
