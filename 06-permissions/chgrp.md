# chgrp - Change Group Ownership

> chgrp (Change Group) is the Linux command used to modify the group ownership of files and directories.

Groups are one of the most important concepts in Linux administration.

Without groups:

```text
Every permission would need to be assigned
to every user individually.
```

Groups allow Linux to manage permissions efficiently across:

- Teams
- Departments
- Applications
- Containers
- Services
- Enterprises

---

# Learning Objectives

After reading this guide you will understand:

✅ What groups are

✅ Why groups exist

✅ Primary Groups

✅ Supplementary Groups

✅ Group Ownership

✅ How Linux stores groups

✅ GID (Group ID)

✅ chgrp command

✅ Group-based permission management

✅ Shared directories

✅ SGID directories

✅ Enterprise permission design

✅ Security best practices

✅ Troubleshooting group issues

---

# Why Groups Exist

Imagine a company:

```text
Developers      50 users
DevOps          20 users
Finance         10 users
HR              5 users
Security        15 users
```

Without groups:

```text
Assign permissions
to every user manually
```

This becomes impossible to manage.

Linux solves this with groups.

---

# What is a Group?

A group is a collection of users.

Example:

```text
developers
```

Members:

```text
alice
bob
charlie
john
```

Instead of:

```text
Grant access
to each user
```

Linux allows:

```text
Grant access
to the group
```

---

# Real World Analogy

Imagine an office.

```text
Room: Development Lab
```

Access:

```text
Developers only
```

You don't create keys for every employee.

You assign:

```text
Developer Badge
```

Linux groups work similarly.

---

# How Linux Uses Groups

Every file has:

```text
Owner
Group Owner
Permissions
```

Example:

```bash
-rw-r----- alice developers report.txt
```

Meaning:

```text
Owner:
alice

Group:
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

# Group IDs (GID)

Linux stores groups as numbers.

Example:

```text
developers = GID 1001

admins     = GID 1002

finance    = GID 1003
```

Internally:

```text
Linux uses GIDs
not names
```

---

# Viewing Group Information

Show current groups:

```bash
groups
```

Example:

```text
alice developers docker sudo
```

---

# Detailed User Information

```bash
id
```

Example:

```text
uid=1001(alice)

gid=1001(alice)

groups=1001(alice),1002(developers),1003(docker)
```

---

# Where Groups Are Stored

Linux stores groups in:

```text
/etc/group
```

Example:

```text
developers:x:1002:alice,bob,charlie
```

Fields:

```text
Group Name
Password Placeholder
GID
Members
```

---

# Primary Group

Every user has one primary group.

Example:

```text
alice
```

Primary group:

```text
alice
```

View:

```bash
id alice
```

---

# Why Primary Groups Matter

When a user creates a file:

```bash
touch report.txt
```

Linux assigns:

```text
Owner  = alice

Group  = primary group
```

Automatically.

---

# Example

User:

```text
alice
```

Primary Group:

```text
developers
```

Creates:

```bash
touch app.js
```

Result:

```text
Owner = alice

Group = developers
```

---

# Supplementary Groups

Users can belong to multiple groups.

Example:

```text
alice
```

Groups:

```text
developers

docker

git

admins
```

---

# Why Supplementary Groups Exist

Example:

```text
Developer

Needs:

Source Code Access
Docker Access
Git Access
```

One group is not enough.

---

# Permission Evaluation with Groups

Suppose:

```bash
-rw-r----- alice developers report.txt
```

User:

```text
bob
```

Groups:

```text
developers
```

Linux checks:

```text
Owner?
```

No.

Then:

```text
Group Member?
```

Yes.

Then:

```text
Use Group Permissions
```

---

# What is chgrp?

chgrp changes:

```text
Group Ownership
```

Only.

---

# Basic Syntax

```bash
chgrp GROUP FILE
```

Example:

```bash
chgrp developers report.txt
```

---

# Before

```bash
-rw-r----- alice users report.txt
```

Command:

```bash
chgrp developers report.txt
```

After:

```bash
-rw-r----- alice developers report.txt
```

---

# Verify Group Ownership

```bash
ls -l
```

Example:

```text
alice developers report.txt
```

---

# Recursive Group Changes

Change entire directory tree.

Syntax:

```bash
chgrp -R GROUP DIRECTORY
```

Example:

```bash
chgrp -R developers project/
```

---

# Visualization

Before:

```text
project/
├── app.js
├── config.json
└── src/
```

Group:

```text
users
```

Command:

```bash
chgrp -R developers project
```

After:

```text
developers
```

for every file.

---

# Shared Team Directories

One of the most common uses of groups.

Example:

```text
/opt/company/project
```

Owner:

```text
root
```

Group:

```text
developers
```

Permissions:

```bash
chmod 770 project
```

Result:

```text
All developers can collaborate.
```

---

# Group-Based Access Design

Bad:

```text
Assign permissions individually.
```

Good:

```text
Assign permissions through groups.
```

---

# Enterprise Example

Company:

```text
Finance Team
```

Group:

```text
finance
```

Directory:

```text
/company/finance
```

Permissions:

```bash
chgrp finance /company/finance

chmod 770 /company/finance
```

Only finance team members gain access.

---

# SGID Directories

Problem:

Users create files.

Group ownership changes unexpectedly.

Example:

```text
alice creates file

Group = developers

bob creates file

Group = admins
```

Collaboration breaks.

---

# SGID Solution

Directory:

```bash
chmod g+s project
```

Now:

```text
All new files inherit
directory group ownership.
```

---

# SGID Visualization

Directory:

```text
project/
```

Group:

```text
developers
```

SGID:

```bash
chmod g+s project
```

New file:

```bash
touch app.js
```

Result:

```text
Group = developers
```

Automatically.

---

# Collaboration Workflow

```text
Shared Directory
      │
      ▼
Group Ownership
      │
      ▼
SGID Enabled
      │
      ▼
Consistent Group Access
```

---

# Production Use Cases

---

# Web Servers

Directory:

```text
/var/www/html
```

Group:

```text
www-data
```

Allows web services access.

---

# Git Repositories

Shared repository:

```text
/project/repo
```

Group:

```text
developers
```

All developers contribute.

---

# Docker

Docker socket:

```text
/var/run/docker.sock
```

Group:

```text
docker
```

Members can run Docker commands.

---

# Kubernetes

Cluster administration:

```text
k8s-admins
```

Group controls access.

---

# Database Teams

Directory:

```text
/db/backups
```

Group:

```text
dbadmins
```

Only database administrators gain access.

---

# Security Best Practices

---

# Principle of Least Privilege

Create groups only for required access.

---

# Use Groups Instead of 777

Bad:

```bash
chmod 777 project
```

Good:

```bash
chgrp developers project

chmod 770 project
```

---

# Minimize Group Membership

Do not add users unnecessarily.

---

# Audit Group Membership

Review regularly.

```bash
getent group
```

---

# Use SGID for Shared Projects

Prevents permission chaos.

---

# Common Mistakes

---

## Using 777 Instead of Groups

Bad:

```bash
chmod 777 shared
```

---

## Wrong Group Ownership

Example:

```text
Finance files
```

Group:

```text
developers
```

Data exposure risk.

---

## Forgetting SGID

Results:

```text
Random group ownership
```

in collaborative directories.

---

## Excessive Group Membership

Users gain unnecessary access.

---

# Troubleshooting

---

# User Cannot Access File

Check:

```bash
id username
```

Verify group membership.

---

# Group Ownership Incorrect

Check:

```bash
ls -l
```

Fix:

```bash
chgrp developers file
```

---

# Shared Directory Not Working

Check:

```bash
ls -ld project
```

Verify:

```text
Group Ownership

Permissions

SGID
```

---

# Interview Questions

---

## What does chgrp do?

Changes group ownership.

---

## Difference between chown and chgrp?

chown:

```text
Changes owner
```

chgrp:

```text
Changes group
```

---

## What is a primary group?

Default group assigned to new files.

---

## What is a supplementary group?

Additional group memberships.

---

## Why use groups?

Simplifies permission management.

---

## What is SGID?

Makes new files inherit directory group ownership.

---

# Quick Cheat Sheet

```bash
chgrp developers file.txt

chgrp developers project

chgrp -R developers project

groups

id

getent group developers

ls -l

chmod g+s project
```

---

# Key Takeaways

```text
1. Groups simplify permission management.

2. Every file has a group owner.

3. Linux uses GIDs internally.

4. chgrp changes group ownership.

5. Primary groups affect new files.

6. Supplementary groups provide additional access.

7. SGID is critical for team collaboration.

8. Group-based permissions are safer than 777.

9. Enterprises rely heavily on group-based access control.
```

---

# Next Step

Continue to:

```text
umask.md
```

where you will learn how Linux automatically assigns default permissions to newly created files and directories, how permission masks work internally, and how enterprises enforce secure default permissions.
