# Real-World Linux Permission Case Studies

> Understanding permissions is important. Understanding how permission mistakes cause real outages, security incidents, and production failures is even more important.

This guide presents real-world inspired scenarios that demonstrate how Linux permissions affect:

```text
Web Servers
Databases
Containers
Cloud Infrastructure
DevOps Pipelines
Enterprise Systems
Security Operations
```

Each case study follows:

```text
Problem
Investigation
Root Cause
Solution
Lessons Learned
```

---

# Case Study #1

Web Server Returns 403 Forbidden

---

# Environment

```text
Ubuntu Server

Nginx

Static Website

SELinux Disabled
```

---

# Problem

Users receive:

```text
403 Forbidden
```

---

# Investigation

Check:

```bash
systemctl status nginx
```

Healthy.

---

Check:

```bash
curl localhost
```

Returns:

```text
403 Forbidden
```

---

Check Files

```bash
ls -l /var/www/html
```

Output:

```text
-rw-r--r-- index.html
```

Looks correct.

---

Check Directory

```bash
ls -ld /var/www/html
```

Output:

```text
drwx------
```

---

# Root Cause

Directory lacked:

```text
Execute Permission
```

for nginx.

---

# Visualization

```text
Browser
   │
   ▼
Nginx
   │
   ▼
Directory
   │
   ▼
No Execute Permission
   │
   ▼
403 Forbidden
```

---

# Solution

```bash
chmod 755 /var/www/html
```

---

# Lesson

Always verify:

```text
Directory Permissions
```

not just file permissions.

---

# Case Study #2

Application Cannot Write Logs

---

# Environment

```text
Node.js

Systemd

Ubuntu
```

---

# Problem

Application crashes.

Error:

```text
Permission Denied
```

writing:

```text
/var/log/app.log
```

---

# Investigation

Service User:

```bash
ps aux
```

Output:

```text
appuser
```

---

File Ownership:

```bash
ls -l /var/log/app.log
```

Output:

```text
root root
```

---

# Root Cause

Application user:

```text
appuser
```

did not own log file.

---

# Visualization

```text
Application
      │
      ▼
appuser
      │
      ▼
root-owned file
      │
      ▼
DENIED
```

---

# Solution

```bash
chown appuser:appuser /var/log/app.log
```

---

# Lesson

Ownership is often more important than permissions.

---

# Case Study #3

Docker Container Cannot Write Volume

---

# Environment

```text
Docker

Ubuntu

Volume Mount
```

---

# Problem

Container starts.

Application fails.

Error:

```text
Permission Denied
```

---

# Investigation

Inside Container:

```bash
id
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

# Root Cause

UID mismatch.

---

# Visualization

```text
Container UID
      │
      ▼
1000
      │
      ▼
Host File
      │
      ▼
2000
      │
      ▼
DENIED
```

---

# Solution

Align:

```text
UIDs
```

or:

```text
Ownership
```

---

# Lesson

Containers use Linux permissions too.

---

# Case Study #4

Shared Team Directory Chaos

---

# Environment

```text
Development Team

20 Engineers

Shared Repository
```

---

# Problem

Developers cannot modify each other's files.

---

# Investigation

```bash
ls -l
```

Files show:

```text
frontend

backend

analytics

mobile
```

different groups.

---

# Root Cause

SGID missing.

---

# Visualization

Without SGID:

```text
Alice
  │
  ▼
frontend
```

```text
Bob
  │
  ▼
backend
```

---

# Solution

```bash
chmod 2775 project
```

---

# Lesson

Shared projects should use:

```text
SGID
```

---

# Case Study #5

Accidental Data Deletion

---

# Environment

```text
University Lab

100 Students

Shared Workspace
```

---

# Problem

Students delete each other's files.

---

Directory:

```text
777
```

---

# Root Cause

Sticky Bit missing.

---

# Visualization

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

# Solution

```bash
chmod 1777 workspace
```

---

# Lesson

Shared writable directories need:

```text
Sticky Bit
```

---

# Case Study #6

Database Backup Leak

---

# Environment

```text
Production MySQL

Shared Server
```

---

# Problem

Developers access database backups.

---

# Investigation

Backup:

```text
backup.sql
```

Permissions:

```text
644
```

---

Created Under:

```bash
umask 022
```

---

# Root Cause

Weak umask.

---

# Visualization

```text
Backup File
      │
      ▼
World Readable
      │
      ▼
Data Exposure
```

---

# Solution

```bash
umask 077
```

---

# Lesson

umask affects security.

---

# Case Study #7

SELinux Blocks Website

---

# Environment

```text
RHEL

Apache

SELinux Enabled
```

---

# Problem

Website returns:

```text
403 Forbidden
```

---

Permissions:

```text
Correct
```

Ownership:

```text
Correct
```

---

# Investigation

```bash
ls -Z
```

Output:

```text
user_home_t
```

---

Expected:

```text
httpd_sys_content_t
```

---

# Root Cause

Wrong SELinux label.

---

# Visualization

```text
Apache
      │
      ▼
SELinux Policy
      │
      ▼
Wrong Context
      │
      ▼
DENIED
```

---

# Solution

```bash
restorecon -Rv /var/www/html
```

---

# Lesson

Permissions may be correct.

SELinux may still deny access.

---

# Case Study #8

Application Works After Disabling SELinux

---

# Problem

Admin runs:

```bash
setenforce 0
```

Application works.

---

Admin conclusion:

```text
Disable SELinux Forever
```

---

Months Later

Server compromised.

---

# Root Cause

Misconfigured context.

Not SELinux itself.

---

# Lesson

Never use:

```text
Disable Security
```

as the first fix.

---

# Case Study #9

Privilege Escalation Through SUID

---

# Environment

```text
Legacy Internal Tool
```

---

# Problem

Program owned by:

```text
root
```

with:

```text
SUID
```

---

Contains:

```c
system(user_input);
```

---

# Attack

User executes:

```bash
/bin/bash
```

---

# Result

```text
Root Shell
```

---

# Visualization

```text
User
 │
 ▼
SUID Program
 │
 ▼
Command Injection
 │
 ▼
Root Access
```

---

# Lesson

Audit SUID binaries regularly.

---

# Case Study #10

ACL Causes Unexpected Access

---

# Problem

File:

```text
640
```

---

Developer:

```text
Can still read file.
```

---

# Investigation

```bash
getfacl file
```

Output:

```text
user:bob:r--
```

---

# Root Cause

ACL granted access.

---

# Visualization

```text
Traditional Permissions
        │
        ▼
ACL Override
        │
        ▼
Access Granted
```

---

# Lesson

Always check:

```bash
getfacl
```

---

# Case Study #11

Capability Missing

---

# Problem

Application fails to bind:

```text
Port 80
```

---

Not running as root.

---

# Investigation

```bash
getcap app
```

No capability.

---

# Root Cause

Missing:

```text
CAP_NET_BIND_SERVICE
```

---

# Solution

```bash
setcap cap_net_bind_service=+ep app
```

---

# Lesson

Capabilities replace many root requirements.

---

# Case Study #12

NFS Permission Nightmare

---

# Environment

```text
NFS Server
```

---

Server UID:

```text
1001
```

---

Client UID:

```text
2000
```

---

# Problem

Access denied.

---

# Visualization

```text
Server UID
      │
      ▼
1001
```

```text
Client UID
      │
      ▼
2000
```

---

Mismatch.

---

# Solution

Synchronize:

```text
UIDs

GIDs
```

---

# Lesson

NFS trusts numeric IDs.

Not usernames.

---

# Master Case Study Flow

Every permission incident follows:

```text
Failure
   │
   ▼
Observation
   │
   ▼
Investigation
   │
   ▼
Root Cause
   │
   ▼
Fix
   │
   ▼
Prevention
```

---

# Permission Investigation Checklist

```text
✓ File Exists

✓ Ownership

✓ Permissions

✓ Parent Directory

✓ Group Membership

✓ ACL

✓ Capabilities

✓ SELinux

✓ AppArmor

✓ Mount Options

✓ Container IDs

✓ NFS Mapping
```

---

# Common Patterns

Most outages are caused by:

```text
Wrong Ownership

Directory Permissions

SELinux Labels

Container UID Mismatch

Missing SGID

Missing Sticky Bit

ACL Confusion
```

Not by:

```text
chmod itself
```

---

# Visual Summary

Linux Security Layers

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
Filesystem
```

Any layer can fail.

---

# Key Takeaways

1. Most permission issues are configuration issues.

2. Ownership problems are extremely common.

3. Directory permissions are often forgotten.

4. Containers introduce UID/GID complexity.

5. SELinux causes many enterprise incidents.

6. ACLs frequently create hidden permissions.

7. SUID binaries require auditing.

8. SGID is essential for collaboration.

9. Sticky Bit protects shared workspaces.

10. Systematic investigation always beats guesswork.

---

# Next Step

Continue to:

```text
permission-flowcharts.md
```

This file will provide large visual troubleshooting diagrams, access-decision trees, ownership resolution maps, ACL evaluation flowcharts, SELinux decision flows, and complete Linux permission architecture diagrams for quick reference and interviews.
