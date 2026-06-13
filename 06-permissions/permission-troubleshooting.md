# Linux Permission Troubleshooting

> Permission troubleshooting is the systematic process of identifying why Linux denied access to a file, directory, process, device, network resource, or service.

Most Linux administrators eventually discover:

```text
Permissions are rarely
the actual problem.
```

A "Permission Denied" error can be caused by:

```text
Traditional Permissions
Ownership
Groups
ACLs
SUID
SGID
Sticky Bit
Capabilities
SELinux
AppArmor
Filesystem Mount Options
Containers
NFS
Samba
Systemd
```

This guide teaches a professional troubleshooting methodology.

---

# Learning Objectives

After reading this guide you will understand:

✅ Permission Denied Workflow

✅ Linux Access Evaluation Order

✅ File Permission Issues

✅ Directory Permission Issues

✅ Ownership Problems

✅ ACL Problems

✅ SELinux Denials

✅ AppArmor Denials

✅ Capability Issues

✅ Container Permission Problems

✅ NFS Permission Problems

✅ Enterprise Troubleshooting Techniques

---

# Golden Rule

Never assume:

```text
chmod 777
```

is the solution.

Most beginners do:

```bash
chmod 777 everything
```

This:

```text
Creates security risks
while often failing to solve the problem.
```

---

# Linux Permission Layers

Linux evaluates access through multiple layers.

Visualization:

```text
Application
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

# Troubleshooting Strategy

Always move:

```text
Simple
   ↓
Complex
```

Never start with:

```text
SELinux
```

unless evidence points there.

---

# Master Troubleshooting Flow

```text
Permission Denied
        │
        ▼
Check File Exists
        │
        ▼
Check Ownership
        │
        ▼
Check Permissions
        │
        ▼
Check Groups
        │
        ▼
Check ACLs
        │
        ▼
Check Mount Options
        │
        ▼
Check SELinux
        │
        ▼
Check AppArmor
        │
        ▼
Check Containers
        │
        ▼
Root Cause Found
```

---

# Step 1

Verify File Exists

Example:

```bash
ls -l file.txt
```

Sometimes:

```text
Wrong filename
Wrong path
Broken symlink
```

creates misleading errors.

---

# Example

Wrong:

```bash
cat report.tx
```

Output:

```text
No such file
```

Not permission issue.

---

# Step 2

Check Ownership

View:

```bash
ls -l file.txt
```

Example:

```bash
-rw------- root root file.txt
```

---

Question:

```text
Who owns the file?
```

---

Visualization:

```text
file.txt

Owner = root

Group = root
```

---

# Common Problem

Application runs as:

```text
nginx
```

File owned by:

```text
root
```

---

Result:

```text
Permission Denied
```

---

# Step 3

Check Permissions

Example:

```bash
ls -l file.txt
```

Output:

```bash
-rw------- root root file.txt
```

---

Permission Analysis

```text
Owner  -> rw-

Group  -> ---

Others -> ---
```

---

Visualization

```text
Current User
      │
      ▼
Matches Owner?
      │
   No
      │
      ▼
Matches Group?
      │
   No
      │
      ▼
Others Permission
      │
      ▼
DENIED
```

---

# Step 4

Check Directory Permissions

Many admins forget this.

---

Example:

```bash
cat /secure/data.txt
```

File:

```text
644
```

Looks fine.

---

Directory:

```text
700
```

Problem.

---

Visualization

```text
Directory
   │
   ▼
No Execute Permission
   │
   ▼
Cannot Reach File
```

---

# Directory Access Rule

To access:

```text
/path/file.txt
```

You need:

```text
Execute permission
```

on:

```text
/
path
file parent
```

directories.

---

# Step 5

Check Group Membership

Current User:

```bash
id
```

Example:

```text
uid=1001(alice)

groups=developers,docker
```

---

File:

```text
Group = finance
```

Problem:

```text
alice not in finance
```

---

Visualization

```text
User
  │
  ▼
Group Check
  │
  ▼
Not Member
  │
  ▼
DENIED
```

---

# Step 6

Check ACLs

Look for:

```bash
ls -l
```

Output:

```text
-rw-r-----+
```

Notice:

```text
+
```

ACL exists.

---

View ACL:

```bash
getfacl file.txt
```

---

Common ACL Problem

```text
ACL grants access

Mask removes access
```

---

Example

```text
user:bob:rwx

mask:r--
```

Effective:

```text
r--
```

Only.

---

Visualization

```text
ACL Entry
      │
      ▼
Mask Applied
      │
      ▼
Effective Permission
```

---

# Step 7

Check Filesystem Mount Options

View:

```bash
mount
```

---

Common Problems

```text
nosuid

noexec

nodev
```

---

# noexec Example

Script:

```bash
./backup.sh
```

Permissions:

```text
755
```

Still fails.

---

Reason:

```text
Filesystem mounted noexec
```

---

Visualization

```text
Script
   │
   ▼
Permission OK
   │
   ▼
Mount Option
   │
   ▼
DENIED
```

---

# Step 8

Check Capabilities

Program:

```text
Bind Port 80
```

Fails.

---

Check:

```bash
getcap binary
```

---

Expected:

```text
cap_net_bind_service
```

---

Missing:

```text
Permission Denied
```

---

# Step 9

Check SELinux

Most common enterprise issue.

---

Check:

```bash
getenforce
```

---

View Labels:

```bash
ls -Z
```

---

Example

```text
httpd_t
```

tries accessing:

```text
shadow_t
```

---

Result:

```text
Denied
```

---

View Denials

```bash
ausearch -m avc
```

---

Visualization

```text
Application
      │
      ▼
SELinux Policy
      │
      ▼
DENY
```

---

# Step 10

Check AppArmor

Ubuntu systems.

---

Status:

```bash
aa-status
```

---

Logs:

```bash
journalctl -xe
```

---

Visualization

```text
Application
      │
      ▼
AppArmor Profile
      │
      ▼
DENY
```

---

# Step 11

Check Containers

Container permissions differ from host.

---

Example

Container User:

```text
1000
```

Host File:

```text
2000
```

---

Volume Mount:

```text
Permission Denied
```

---

Visualization

```text
Container UID
      │
      ▼
Volume Ownership
      │
      ▼
Mismatch
      │
      ▼
DENIED
```

---

# Step 12

Check NFS

Very common enterprise issue.

---

Server:

```text
alice = UID 1001
```

---

Client:

```text
alice = UID 2000
```

---

Result:

```text
Ownership mismatch
```

---

Visualization

```text
NFS Server
     │
     ▼
UID Mapping
     │
     ▼
Client
     │
     ▼
Permission Failure
```

---

# Step 13

Check Samba

Windows permissions.

---

Samba may translate:

```text
Windows ACLs
```

into:

```text
Linux ACLs
```

Unexpected access failures occur.

---

# Real Production Scenario #1

Web Server Cannot Read Website

---

Error

```text
403 Forbidden
```

---

Investigation

```bash
ls -l /var/www
```

Looks correct.

---

Check:

```bash
ls -Z
```

Wrong SELinux context.

---

Fix:

```bash
restorecon -Rv /var/www
```

---

Root Cause:

```text
SELinux Label
```

Not permissions.

---

# Real Production Scenario #2

Docker Application Cannot Write

---

Error:

```text
Permission Denied
```

---

Check:

```bash
docker exec app id
```

Output:

```text
uid=1000
```

---

Host:

```bash
ls -ln data
```

Output:

```text
2000:2000
```

---

Problem:

```text
UID mismatch
```

---

# Real Production Scenario #3

Shared Team Directory Broken

---

Directory:

```text
/project
```

---

Files:

```text
frontend
backend
```

mixed groups.

---

Problem:

```text
SGID missing
```

---

Fix:

```bash
chmod 2775 project
```

---

# Common Mistakes

---

## Immediately Using 777

Bad:

```bash
chmod 777 file
```

---

## Disabling SELinux

Bad:

```bash
setenforce 0
```

Permanent disablement not recommended.

---

## Running Everything as Root

Bad:

```text
Security Risk
```

---

## Ignoring Directory Permissions

Very common.

---

# Professional Diagnostic Commands

Ownership:

```bash
ls -l
```

---

Detailed:

```bash
stat file
```

---

Groups:

```bash
id
```

---

ACL:

```bash
getfacl file
```

---

SELinux:

```bash
ls -Z

getenforce

ausearch -m avc
```

---

AppArmor:

```bash
aa-status
```

---

Capabilities:

```bash
getcap file
```

---

Filesystem:

```bash
mount
```

---

# Universal Troubleshooting Flowchart

```text
Permission Denied
        │
        ▼
File Exists?
        │
        ▼
Ownership OK?
        │
        ▼
Permissions OK?
        │
        ▼
Directory Access OK?
        │
        ▼
Group Membership OK?
        │
        ▼
ACL OK?
        │
        ▼
Filesystem OK?
        │
        ▼
Capabilities OK?
        │
        ▼
SELinux OK?
        │
        ▼
AppArmor OK?
        │
        ▼
Container/NFS Issue?
        │
        ▼
ROOT CAUSE FOUND
```

---

# Interview Questions

---

## What is the first thing you check?

```text
File existence
```

---

## Why can a 644 file still fail?

Directory permissions.

---

## What does a "+" in ls mean?

ACL exists.

---

## Which command checks ACLs?

```bash
getfacl
```

---

## Which command checks SELinux context?

```bash
ls -Z
```

---

## Why might a 755 script fail to execute?

```text
noexec mount
```

---

## Why might root-owned files fail?

```text
SELinux
AppArmor
```

---

# Quick Cheat Sheet

```bash
ls -l

stat file

id

groups

getfacl file

mount

getcap file

getenforce

ls -Z

ausearch -m avc

aa-status
```

---

# Key Takeaways

1. Permission errors have many causes.

2. Always follow a structured workflow.

3. Ownership is checked before permissions.

4. Directory permissions matter.

5. ACL masks frequently cause confusion.

6. SELinux is often the real cause.

7. AppArmor can override permissions.

8. Containers introduce UID/GID complexity.

9. NFS and Samba create identity mapping issues.

10. Professional troubleshooting follows a systematic process, not trial and error.

---

# Next Step

Continue to:

```text
security-hardening.md
```

This file will teach how to build a production-grade Linux system using:

- Least Privilege
- Secure umask policies
- SUID auditing
- Capability minimization
- ACL strategy
- SELinux/AppArmor enforcement
- Container hardening
- Enterprise security baselines
- CIS Benchmark concepts
- Real-world attack prevention
