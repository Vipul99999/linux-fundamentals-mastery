# chown - Change File Ownership

> chown (Change Owner) is the Linux command used to modify the ownership of files and directories.

Ownership is one of the most important concepts in Linux security.

Before Linux checks permissions, it first determines:

```text
Who owns the resource?
```

Only after ownership is known can Linux decide:

```text
Which permissions apply?
```

Understanding ownership is essential for:

- Linux Administration
- DevOps
- Cloud Engineering
- Cybersecurity
- Containers
- Kubernetes
- Storage Management
- Production Server Operations

---

# Learning Objectives

After reading this guide you will understand:

✅ What ownership means

✅ User Ownership

✅ Group Ownership

✅ How Linux stores ownership

✅ UID and GID

✅ chown command syntax

✅ Recursive ownership changes

✅ Ownership in production environments

✅ Docker ownership problems

✅ NFS ownership issues

✅ Security best practices

✅ Troubleshooting ownership problems

---

# Why Ownership Exists

Imagine a multi-user server:

```text
alice
bob
charlie
nginx
mysql
postgres
backup
```

Without ownership:

```text
Anyone could modify anyone else's files.
```

Linux solves this by assigning every file an owner.

---

# Ownership Fundamentals

Every file has:

```text
1. User Owner
2. Group Owner
```

Example:

```bash
-rw-r----- alice developers report.txt
```

Owner:

```text
alice
```

Group:

```text
developers
```

---

# Ownership Visualization

```text
-rw-r----- alice developers report.txt
            │        │
            │        └── Group Owner
            │
            └────────── User Owner
```

---

# Ownership and Permissions

Permissions depend on ownership.

Example:

```bash
-rwx------ alice developers secret.sh
```

Linux checks:

```text
Are you alice?
```

If yes:

```text
Owner permissions apply
```

If no:

```text
Group permissions checked
```

---

# How Linux Stores Ownership

Linux stores ownership inside the inode.

---

# Inode Structure

```text
File
 │
 ▼
Inode
 │
 ├── UID
 ├── GID
 ├── Permissions
 ├── Timestamps
 └── Metadata
```

---

# UID (User ID)

Linux uses numbers internally.

Example:

```text
alice = UID 1001
bob   = UID 1002
root  = UID 0
```

View:

```bash
id alice
```

Example:

```bash
uid=1001(alice)
```

---

# GID (Group ID)

Groups also have IDs.

Example:

```text
developers = GID 1010
```

View:

```bash
getent group developers
```

---

# Viewing Ownership

Use:

```bash
ls -l
```

Example:

```bash
-rw-r----- alice developers report.txt
```

---

# Detailed Ownership Information

Use:

```bash
stat report.txt
```

Example:

```text
Uid: (1001/alice)
Gid: (1010/developers)
```

---

# What is chown?

chown changes:

```text
User Owner
Group Owner
Both
```

---

# Basic Syntax

```bash
chown OWNER FILE
```

Example:

```bash
chown alice report.txt
```

New owner:

```text
alice
```

---

# Change Owner

Example:

```bash
chown bob notes.txt
```

Before:

```text
Owner = alice
```

After:

```text
Owner = bob
```

---

# Change Owner and Group

Syntax:

```bash
chown USER:GROUP FILE
```

Example:

```bash
chown alice:developers app.js
```

Result:

```text
Owner = alice

Group = developers
```

---

# Change Group Only

Example:

```bash
chown :developers report.txt
```

Only group changes.

---

# Visual Example

Before:

```bash
-rw-r--r-- alice users report.txt
```

Command:

```bash
chown bob:admins report.txt
```

After:

```bash
-rw-r--r-- bob admins report.txt
```

---

# Recursive Ownership Changes

Change entire directory tree.

Syntax:

```bash
chown -R USER:GROUP DIRECTORY
```

Example:

```bash
chown -R alice:developers project/
```

---

# Recursive Visualization

Before:

```text
project/
├── app.js
├── config.json
└── src/
```

Ownership:

```text
root:root
```

Command:

```bash
chown -R alice:developers project
```

After:

```text
alice:developers
```

for every file and folder.

---

# Verify Ownership

```bash
ls -l
```

Or:

```bash
find project -ls
```

---

# Ownership of Directories

Directories also have owners.

Example:

```bash
drwxr-x--- alice developers project/
```

Owner controls:

```text
Creation
Deletion
Modification
```

inside the directory.

---

# Symbolic Links

Special behavior:

```bash
ln -s target link
```

Default:

```bash
chown user link
```

changes target ownership.

---

# Ownership by UID

Instead of names:

```bash
chown 1001 file.txt
```

Linux resolves:

```text
UID 1001
```

to user.

---

# Ownership by UID:GID

Example:

```bash
chown 1001:1010 file.txt
```

---

# Production Use Cases

---

# Web Server

Directory:

```text
/var/www/html
```

Ownership:

```bash
root:www-data
```

Example:

```bash
chown -R root:www-data /var/www/html
```

Reason:

```text
Web server can read files.

Normal users cannot modify them.
```

---

# Nginx Logs

```text
/var/log/nginx
```

Ownership:

```bash
chown -R nginx:nginx /var/log/nginx
```

Allows logging.

---

# MySQL

Database files:

```text
/var/lib/mysql
```

Ownership:

```bash
mysql:mysql
```

Required.

---

# PostgreSQL

Data directory:

```text
/var/lib/postgresql
```

Ownership:

```bash
postgres:postgres
```

Required.

---

# SSH

Home directory:

```text
/home/alice
```

Ownership:

```bash
alice:alice
```

Required for proper SSH operation.

---

# Docker Ownership Problems

Very common issue.

Container:

```text
UID 1000
```

Host:

```text
UID 1001
```

Result:

```text
Permission Denied
```

because ownership does not match.

---

# Example Docker Issue

Volume:

```bash
-v /host/data:/app/data
```

Container tries:

```text
Write File
```

Fails because:

```text
Ownership mismatch
```

---

# Kubernetes Ownership

Pods often use:

```yaml
securityContext:
  runAsUser: 1000
```

Volume ownership must match.

Otherwise:

```text
Permission Denied
```

---

# NFS Ownership Problems

NFS uses UID/GID mapping.

Example:

Server:

```text
alice = UID 1001
```

Client:

```text
alice = UID 2000
```

Result:

```text
Ownership mismatch
```

Access issues occur.

---

# Security Implications

Ownership is often more important than permissions.

Example:

```bash
-rwxrwxrwx script.sh
```

If attacker owns it:

```text
They control the file.
```

---

# Common Security Mistakes

---

## Running chown Recursively on Wrong Directory

Bad:

```bash
chown -R alice /
```

Disaster.

Can break entire system.

---

## Changing System Ownership

Bad:

```bash
chown alice /bin/bash
```

Can damage OS integrity.

---

## Wrong Service Ownership

Example:

```text
Nginx files owned by root only
```

Result:

```text
Cannot write logs
```

---

## Using Root for Everything

Bad:

```text
All application files owned by root
```

Applications fail.

---

# Advanced chown Options

---

# Verbose Output

```bash
chown -v alice file.txt
```

Shows changes.

---

# Change Only When Needed

```bash
chown -c alice file.txt
```

Displays actual modifications.

---

# Reference Ownership

Copy ownership.

```bash
chown --reference=file1 file2
```

Useful for automation.

---

# Preserve Root

Safety feature:

```bash
chown --preserve-root -R ...
```

Prevents dangerous recursive operations.

---

# Ownership Troubleshooting

---

# Permission Denied

Check:

```bash
ls -l
```

Verify owner.

---

# Service Cannot Write Files

Check:

```bash
ps aux
```

Find service user.

Verify ownership.

---

# Web Server Errors

Check:

```bash
ls -l /var/www
```

Ownership often incorrect.

---

# Docker Permission Errors

Check:

```bash
id
```

inside container.

Compare with host ownership.

---

# Interview Questions

---

## What does chown do?

Changes file ownership.

---

## Difference between chmod and chown?

chmod:

```text
Changes permissions.
```

chown:

```text
Changes ownership.
```

---

## What does chown user:group file mean?

Changes:

```text
Owner
and
Group
```

---

## What does chown -R do?

Recursive ownership change.

---

## Why is ownership important?

Permissions are evaluated based on ownership.

---

# Quick Cheat Sheet

```bash
chown alice file.txt

chown alice:developers file.txt

chown :developers file.txt

chown -R alice project/

chown -v alice file.txt

chown -c alice file.txt

chown --reference=file1 file2
```

---

# Key Takeaways

```text
1. Every file has an owner.

2. Every file has a group owner.

3. Ownership is stored in the inode.

4. Linux checks ownership before permissions.

5. chown changes ownership.

6. Recursive ownership changes require caution.

7. Ownership problems are a common cause of:
   - Web server failures
   - Docker issues
   - Kubernetes volume errors
   - NFS access problems

8. Ownership is a core Linux security mechanism.
```

---

# Next Step

Continue to:

```text
chgrp.md
```

where you will learn Linux group ownership, supplementary groups, group inheritance, SGID directories, collaborative environments, enterprise group management, and production permission design.
