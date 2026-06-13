# umask - Default Permission Control

> umask (User File Creation Mask) controls the default permissions assigned to newly created files and directories.

It is one of Linux's most important security mechanisms.

Most people think:

```text
chmod changes permissions.
```

True.

But before chmod is ever used:

```text
Linux already assigned permissions.
```

The component responsible for those default permissions is:

```text
umask
```

---

# Learning Objectives

After reading this guide you will understand:

✅ What umask is

✅ Why Linux needs umask

✅ How files are created internally

✅ Default file permissions

✅ Default directory permissions

✅ Permission masking

✅ Numeric umask values

✅ Symbolic umask values

✅ User-level configuration

✅ System-wide configuration

✅ Service-specific umask

✅ Security best practices

✅ Enterprise usage

✅ Troubleshooting

---

# Why umask Exists

Imagine Linux creates files like this:

```text
File:
rwxrwxrwx (777)

Directory:
rwxrwxrwx (777)
```

This would be a disaster.

Every new file would be:

```text
Readable by everyone

Writable by everyone

Executable by everyone
```

Huge security risk.

Linux solves this using:

```text
umask
```

---

# The Purpose of umask

umask removes permissions during creation.

```text
Default Permissions
        │
        ▼
Apply umask
        │
        ▼
Final Permissions
```

---

# Important Rule

umask does NOT:

```text
Grant permissions
```

umask only:

```text
Removes permissions
```

Think of it as:

```text
Permission Filter
```

---

# File Creation Process

When a program creates a file:

```text
Application
      │
      ▼
Kernel
      │
      ▼
Base Permission
      │
      ▼
Apply umask
      │
      ▼
Final Permission
```

---

# Base Permissions

Linux starts with:

---

## Files

```text
666

rw-rw-rw-
```

Notice:

```text
No Execute Bit
```

Linux assumes:

```text
Most files are data files.
```

---

## Directories

```text
777

rwxrwxrwx
```

Directories require execute permission.

---

# Why Files Start With 666

Security.

Example:

```text
document.txt
```

Should not automatically become executable.

Therefore:

```text
Files start at 666
```

Not:

```text
777
```

---

# Why Directories Start With 777

Directories require:

```text
Read
Write
Execute
```

to function properly.

Therefore:

```text
Directories start at 777
```

---

# Understanding the Mask

Suppose:

```text
umask = 022
```

---

# New File Calculation

Base:

```text
666
```

Mask:

```text
022
```

Result:

```text
644
```

Visual:

```text
666
022
---
644
```

Final:

```text
rw-r--r--
```

---

# New Directory Calculation

Base:

```text
777
```

Mask:

```text
022
```

Result:

```text
755
```

Visual:

```text
777
022
---
755
```

Final:

```text
rwxr-xr-x
```

---

# Common umask Values

---

## 022

Most common.

```text
Files:       644
Directories: 755
```

Used by:

```text
Most Linux distributions
```

---

## 027

More secure.

```text
Files:       640
Directories: 750
```

Group access only.

---

## 077

Private environment.

```text
Files:       600
Directories: 700
```

Owner only.

---

## 002

Collaborative environment.

```text
Files:       664
Directories: 775
```

Common on development servers.

---

# Visual Comparison

```text
UMASK   FILE   DIRECTORY

022     644    755
002     664    775
027     640    750
077     600    700
```

---

# Viewing Current umask

Display current value:

```bash
umask
```

Example:

```bash
022
```

---

# Symbolic Output

```bash
umask -S
```

Example:

```text
u=rwx,g=rx,o=rx
```

---

# Changing umask Temporarily

Current shell only.

Example:

```bash
umask 077
```

Verify:

```bash
umask
```

Output:

```text
077
```

---

# Testing umask

Set:

```bash
umask 077
```

Create file:

```bash
touch secret.txt
```

Check:

```bash
ls -l
```

Result:

```text
-rw-------
```

---

# Testing Directory Creation

Create:

```bash
mkdir private
```

Check:

```bash
ls -ld private
```

Result:

```text
drwx------
```

---

# Temporary vs Permanent

Temporary:

```bash
umask 027
```

Only affects current shell.

---

# Permanent Configuration

User level:

```text
~/.bashrc
~/.profile
~/.zshrc
```

Example:

```bash
umask 027
```

---

# System-Wide Configuration

Files:

```text
/etc/profile
/etc/bash.bashrc
/etc/login.defs
```

Example:

```bash
UMASK 027
```

---

# Verify Login Settings

```bash
grep UMASK /etc/login.defs
```

Example:

```text
UMASK 022
```

---

# Enterprise Permission Flow

```text
User Login
      │
      ▼
Shell Startup
      │
      ▼
Read umask
      │
      ▼
Create Files
      │
      ▼
Apply Security Policy
```

---

# Shared Development Environment

Team:

```text
Developers
```

Need:

```text
Shared access
```

Use:

```text
umask 002
```

Results:

```text
Files: 664

Directories: 775
```

Group collaboration works.

---

# Secure Server Environment

Need:

```text
Maximum Privacy
```

Use:

```text
umask 077
```

Results:

```text
Files: 600

Directories: 700
```

---

# Service-Specific umask

Services can have their own umask.

Example:

```text
nginx
mysql
postgres
docker
```

---

# Systemd Example

View:

```bash
systemctl cat nginx
```

Possible:

```ini
UMask=0027
```

Meaning:

```text
Nginx-created files
use 0027
```

---

# Why Services Use Custom umask

Example:

```text
Web logs
```

Should not be:

```text
World readable
```

Custom umask solves this.

---

# Docker and Containers

Containers also use umask.

Example:

```bash
docker exec container umask
```

Output:

```text
0022
```

Container-created files follow that mask.

---

# Kubernetes

Pods inherit:

```text
Container umask
```

Improper configuration can expose:

```text
Secrets
ConfigMaps
Application Data
```

---

# Security Implications

Weak umask:

```text
002
```

May expose files.

Strong umask:

```text
077
```

Protects sensitive data.

---

# Real Security Incident

Developer creates:

```text
database_backup.sql
```

Server:

```text
umask 002
```

Result:

```text
Other users can read backup.
```

Database leak occurs.

---

# Best Practices

---

## Personal Workstations

Recommended:

```text
022
```

---

## Shared Teams

Recommended:

```text
002
```

or

```text
027
```

---

## Production Servers

Recommended:

```text
027
```

or

```text
077
```

---

## Sensitive Systems

Recommended:

```text
077
```

---

# Common Mistakes

---

## Confusing umask with chmod

Wrong:

```text
umask changes existing files
```

False.

Only affects:

```text
New files
```

---

## Using 000

Example:

```bash
umask 000
```

Result:

```text
Maximum exposure
```

Dangerous.

---

## Forgetting Service umask

Application creates:

```text
Sensitive Logs
```

Unexpected permissions appear.

---

## Assuming Existing Files Change

They do not.

Only new files use the new mask.

---

# Troubleshooting

---

## Wrong Default Permissions

Check:

```bash
umask
```

---

## Service Creates Wrong Permissions

Check:

```bash
systemctl cat service
```

Look for:

```ini
UMask=
```

---

## New Files Too Restrictive

Current umask may be:

```text
077
```

---

## New Files Too Open

Current umask may be:

```text
000
002
```

---

# Interview Questions

---

## What is umask?

A permission mask used during file creation.

---

## Does umask add permissions?

No.

Only removes permissions.

---

## Why do files start at 666?

Security.

Files should not automatically be executable.

---

## Why do directories start at 777?

Directories require execute permission.

---

## What does umask 022 produce?

Files:

```text
644
```

Directories:

```text
755
```

---

## Difference between chmod and umask?

chmod:

```text
Changes existing permissions.
```

umask:

```text
Controls default permissions for new files.
```

---

# Quick Cheat Sheet

```bash
umask

umask -S

umask 022

umask 027

umask 077

touch file.txt

mkdir test

grep UMASK /etc/login.defs
```

---

# Visual Summary

```text
Create File
     │
     ▼
Base Permission
     │
     ▼
666 (File)
777 (Directory)
     │
     ▼
Apply umask
     │
     ▼
Final Permission
```

---

# Key Takeaways

1. umask controls default permissions.

2. Files start at 666.

3. Directories start at 777.

4. umask removes permissions.

5. umask affects only new files.

6. 022 is the most common setting.

7. 077 is the most restrictive common setting.

8. Services can have their own umask.

9. Proper umask configuration is critical for security.

10. Enterprise environments rely heavily on secure umask policies.

---

# Next Step

Continue to:

```text
suid.md
```

You will learn one of the most powerful and dangerous Linux permission mechanisms: **Set User ID (SUID)**, how programs temporarily gain elevated privileges, how tools like `passwd` work, how attackers abuse SUID binaries, and how administrators secure them.
