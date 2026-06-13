# Linux Permission Model

> The Linux Permission Model is the security framework that determines who can access files, directories, devices, processes, and system resources.

Understanding this model is critical because every Linux security mechanism builds on top of it.

---

# Learning Objectives

After reading this document you will understand:

✅ Why permissions exist

✅ How Linux evaluates access requests

✅ Users, Groups, and Others

✅ Ownership

✅ Read, Write, Execute permissions

✅ File permissions

✅ Directory permissions

✅ Permission resolution order

✅ Effective permissions

✅ Root privileges

✅ Security layers beyond traditional permissions

✅ Real-world production permission design

---

# Why Linux Needs a Permission Model

Imagine a server used by:

```text
alice
bob
charlie
database
nginx
backup-system
```

Without permissions:

```text
Alice could delete Bob's files

Applications could modify system files

Attackers could access sensitive data

Any user could take over the system
```

Linux solves this through a permission model.

---

# The Core Principle

Every access request asks:

```text
WHO
wants to do

WHAT

to

WHICH RESOURCE
```

Example:

```text
User: alice

Action: Read

Resource: report.txt
```

Linux evaluates whether that action is allowed.

---

# High-Level Security Flow

```text
User Request
      │
      ▼
Ownership Check
      │
      ▼
Permission Check
      │
      ▼
ACL Check
      │
      ▼
SELinux/AppArmor
      │
      ▼
Kernel Decision
      │
      ▼
ALLOW / DENY
```

---

# Core Components

Linux permissions are built on three concepts:

```text
1. Users
2. Groups
3. Permissions
```

---

# Users

Every action in Linux runs as a user.

Examples:

```text
root
alice
bob
nginx
mysql
postgres
```

View current user:

```bash
whoami
```

Example:

```bash
alice
```

---

# User IDs (UID)

Internally Linux uses numbers.

Example:

```text
alice = UID 1001

bob = UID 1002

root = UID 0
```

View UID:

```bash
id
```

Output:

```text
uid=1001(alice)
gid=1001(alice)
```

---

# Root User

Special user:

```text
UID 0
```

Root bypasses most permission checks.

Example:

```bash
sudo cat /etc/shadow
```

Normal users:

```text
Access Denied
```

Root:

```text
Access Allowed
```

---

# Groups

Groups simplify permission management.

Instead of:

```text
Give access to 50 users individually
```

Linux allows:

```text
Create one group
Assign permissions to group
Add users to group
```

Example:

```text
developers
admins
docker
finance
```

---

# Group IDs (GID)

Groups also have numeric IDs.

Example:

```text
developers = GID 1005
```

View:

```bash
getent group developers
```

---

# Permission Categories

Every file has permissions for:

```text
Owner
Group
Others
```

---

# Visual Representation

```text
-rwxr-xr--
```

Breakdown:

```text
Owner    Group    Others
rwx      r-x      r--
```

---

# What Is Ownership?

Every file belongs to:

```text
User Owner
Group Owner
```

Example:

```bash
-rw-r----- 1 alice developers report.txt
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

# Permission Types

Linux uses three permission types.

```text
Read
Write
Execute
```

---

# Read Permission (r)

Value:

```text
4
```

Allows:

```text
View file contents
Open file
Copy file
```

Example:

```bash
cat report.txt
```

---

# Write Permission (w)

Value:

```text
2
```

Allows:

```text
Modify file
Append data
Truncate file
```

Example:

```bash
echo hello >> file.txt
```

---

# Execute Permission (x)

Value:

```text
1
```

Allows:

```text
Run program
Execute script
```

Example:

```bash
./script.sh
```

---

# Permission Matrix

| Permission | File | Directory |
|------------|-------|------------|
| Read | View content | List files |
| Write | Modify file | Create/Delete entries |
| Execute | Run file | Enter directory |

---

# Directory Permissions Explained

Most beginners misunderstand directory permissions.

---

## Read on Directory

Allows:

```text
See file names
```

Example:

```bash
ls
```

---

## Write on Directory

Allows:

```text
Create files
Delete files
Rename files
```

---

## Execute on Directory

Allows:

```text
Enter directory
Access contents
```

Example:

```bash
cd projects
```

Without execute:

```text
Permission Denied
```

---

# Example Directory

```text
project/
├── app.js
├── logs
└── config
```

Permissions:

```bash
drwxr-x---
```

Meaning:

```text
Owner:
Full Access

Group:
Read + Execute

Others:
No Access
```

---

# Numeric Permission System

Linux converts permissions into numbers.

```text
Read = 4
Write = 2
Execute = 1
```

---

# Numeric Examples

### 7

```text
4 + 2 + 1

rwx
```

---

### 6

```text
4 + 2

rw-
```

---

### 5

```text
4 + 1

r-x
```

---

### 4

```text
r--
```

---

# Common Permissions

| Numeric | Meaning |
|----------|----------|
| 777 | Full access |
| 755 | Executable files |
| 750 | Private executable |
| 700 | Owner only |
| 644 | Standard file |
| 640 | Restricted file |
| 600 | Sensitive file |

---

# How Linux Decides Access

Suppose:

```bash
-rw-r----- alice developers report.txt
```

User:

```text
bob
```

Group:

```text
developers
```

Request:

```text
Read report.txt
```

Linux follows this order.

---

# Step 1

Is Bob the owner?

```text
No
```

Move to next step.

---

# Step 2

Is Bob in group developers?

```text
Yes
```

Use Group permissions.

---

# Step 3

Group permissions:

```text
r--
```

Read allowed.

Result:

```text
ACCESS GRANTED
```

---

# Permission Resolution Flow

```text
Access Request
       │
       ▼
Owner?
       │
   Yes │ No
       ▼
Owner Permissions
       │
       ▼
Access Decided
```

If not owner:

```text
Group Member?
       │
   Yes │ No
       ▼
Group Permissions
```

If not group:

```text
Use Others Permissions
```

---

# Important Rule

Linux does NOT combine permissions.

Example:

```text
Owner = r--
Group = rw-
Others = ---
```

If user matches owner:

```text
Only owner permissions apply
```

Group permissions are ignored.

---

# Effective Permissions

Actual permissions Linux uses are called:

```text
Effective Permissions
```

These depend on:

```text
Ownership
Group Membership
ACL
Capabilities
SELinux
AppArmor
```

---

# Symbolic Representation

Example:

```bash
-rwxr-xr--
```

Breakdown:

```text
-
│
├── File Type
│
├── rwx Owner
│
├── r-x Group
│
└── r-- Others
```

---

# File Types

First character indicates file type.

| Symbol | Type |
|----------|--------|
| - | Regular File |
| d | Directory |
| l | Symbolic Link |
| c | Character Device |
| b | Block Device |
| p | Named Pipe |
| s | Socket |

Example:

```bash
drwxr-xr-x
```

Directory.

---

# Beyond Traditional Permissions

Modern Linux adds additional layers.

---

# ACL

Traditional model:

```text
Owner
Group
Others
```

Problem:

Need custom permissions.

Solution:

```text
ACL
```

---

# Capabilities

Traditional model:

```text
Root = Everything
```

Modern model:

```text
Split privileges
```

Examples:

```text
CAP_NET_ADMIN

CAP_SYS_ADMIN

CAP_NET_BIND_SERVICE
```

---

# SELinux

Adds:

```text
Mandatory Access Control
```

Even if permissions allow:

```text
SELinux can deny access.
```

---

# AppArmor

Application profile-based security.

Example:

```text
Firefox Profile

Docker Profile

Nginx Profile
```

---

# Real Production Example

Web server:

```text
User:
www-data

Directory:
/var/www/html
```

Permissions:

```text
Owner = root

Group = www-data

Mode = 750
```

Result:

```text
Web server can read files

Normal users cannot
```

---

# Common Security Mistakes

---

## Using 777

Bad:

```bash
chmod 777 file.txt
```

Why?

```text
Anyone can modify the file.
```

---

## Running Everything as Root

Bad:

```bash
sudo su
```

Use least privilege.

---

## Wrong Ownership

Example:

```text
Application logs owned by root
```

Application cannot write logs.

---

# Permission Design Principles

---

## Principle of Least Privilege

Give:

```text
Minimum permissions required
```

Not:

```text
Maximum permissions possible
```

---

## Group-Based Management

Prefer:

```text
Groups
```

Instead of:

```text
User-by-user permission assignment
```

---

## Layered Security

Use:

```text
Permissions
+
ACL
+
SELinux/AppArmor
```

Not permissions alone.

---

# Key Takeaways

```text
1. Linux permissions control access.

2. Every file has:
   Owner
   Group
   Others

3. Permissions are:
   Read
   Write
   Execute

4. Linux checks:
   Owner → Group → Others

5. Root bypasses most checks.

6. Modern Linux also uses:
   ACL
   Capabilities
   SELinux
   AppArmor

7. Least Privilege is the foundation
   of Linux security.
```

---

# Next Step

Continue to:

```text
chmod.md
```

Where you will learn how to modify permissions using symbolic and numeric methods, understand recursive permission changes, secure production systems, and avoid dangerous permission configurations.
