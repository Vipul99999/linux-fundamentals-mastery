# Common Linux Permission Mistakes

> Most Linux permission problems are not caused by Linux itself. They are caused by incorrect assumptions, rushed fixes, and misunderstanding how Linux security works.

Many production outages begin with:

```text
"It was just a permission change."
```

This guide documents the most common permission mistakes made by:

```text
Beginners
System Administrators
DevOps Engineers
Cloud Engineers
Security Teams
Developers
```

and shows how to avoid them.

---

# Learning Objectives

After reading this guide you will understand:

✅ Dangerous Permission Practices

✅ Common Ownership Mistakes

✅ ACL Mistakes

✅ SELinux Mistakes

✅ AppArmor Mistakes

✅ Docker Permission Mistakes

✅ Kubernetes Permission Mistakes

✅ SUID Mistakes

✅ Security Anti-Patterns

✅ Real Production Failures

---

# Mistake #1

Using chmod 777 Everywhere

---

Problem

User sees:

```text
Permission Denied
```

Immediately runs:

```bash
chmod 777 file
```

---

Visualization

```text
Permission Problem
      │
      ▼
chmod 777
      │
      ▼
Security Problem
```

---

Why Dangerous

Everyone gains:

```text
Read

Write

Execute
```

---

Real Risk

```text
File Modification

Malware Placement

Data Theft

Privilege Escalation
```

---

Better Solution

Investigate:

```text
Ownership

Groups

ACLs

SELinux
```

---

# Mistake #2

Using Root For Everything

---

Bad Practice

```bash
sudo su
```

then:

```text
Work Entire Day As Root
```

---

Visualization

```text
Root Shell
    │
    ▼
Small Mistake
    │
    ▼
System Damage
```

---

Real Example

```bash
rm -rf *
```

executed in wrong directory.

Catastrophic.

---

Better

```text
Use sudo only when required.
```

---

# Mistake #3

Recursive chmod on Wrong Directory

---

Dangerous Command

```bash
chmod -R 777 /
```

---

Visualization

```text
Recursive Command
       │
       ▼
Entire Filesystem
       │
       ▼
Destroyed Security
```

---

Consequences

```text
Broken Services

Exposed Data

Security Violations
```

---

# Mistake #4

Recursive chown on Root Filesystem

---

Dangerous

```bash
chown -R user /
```

---

Result

```text
System Files
Ownership Corrupted
```

---

Example

```text
/bin

/etc

/lib
```

become incorrect.

---

Possible Outcome

```text
System Won't Boot
```

---

# Mistake #5

Ignoring Directory Permissions

---

Many beginners check:

```text
File Permissions
```

only.

---

Example

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

---

Visualization

```text
Directory Blocked
       │
       ▼
File Unreachable
```

---

Remember

```text
Directory execute permission
controls traversal.
```

---

# Mistake #6

Wrong Ownership

---

Example

Application:

```text
nginx
```

Runs as:

```text
www-data
```

---

Files Owned By

```text
root
```

---

Result

```text
Cannot Write Logs
```

---

Visualization

```text
Application User
       │
       ▼
Ownership Mismatch
       │
       ▼
Permission Denied
```

---

# Mistake #7

Confusing File Permissions and Directory Permissions

---

File Read

```text
Allows viewing contents
```

---

Directory Read

```text
Allows listing entries
```

---

They are not the same.

---

Visualization

```text
File
 │
 ▼
Read Content
```

```text
Directory
 │
 ▼
Read Filenames
```

---

# Mistake #8

Forgetting umask

---

Problem

New files unexpectedly created:

```text
Too Open

Too Restrictive
```

---

Example

```bash
umask 000
```

Creates:

```text
World Accessible Files
```

---

Visualization

```text
Create File
      │
      ▼
Bad umask
      │
      ▼
Weak Security
```

---

# Mistake #9

Ignoring ACLs

---

File Appears:

```text
640
```

Yet user still accesses it.

---

Reason

```text
ACL grants access
```

---

Indicator

```bash
ls -l
```

Shows:

```text
+
```

---

Visualization

```text
Permissions
      │
      ▼
ACL Exists
      │
      ▼
Different Result
```

---

# Mistake #10

Forgetting ACL Mask

---

ACL

```text
user:bob:rwx
```

---

Mask

```text
r--
```

---

Effective

```text
r--
```

only.

---

Visualization

```text
ACL Entry
      │
      ▼
Mask Applied
      │
      ▼
Reduced Access
```

---

# Mistake #11

Disabling SELinux

---

Bad Fix

```bash
setenforce 0
```

works.

Then:

```bash
SELINUX=disabled
```

---

Visualization

```text
Problem
   │
   ▼
Disable SELinux
   │
   ▼
Security Reduced
```

---

Better

```text
Fix Labels
Fix Policy
```

---

# Mistake #12

Ignoring SELinux Contexts

---

File Permissions

```text
Correct
```

---

Ownership

```text
Correct
```

---

Still fails.

---

Reason

```text
Wrong Label
```

---

Check

```bash
ls -Z
```

---

# Mistake #13

Ignoring AppArmor

---

Ubuntu Users Often Forget

```text
AppArmor Exists
```

---

Application

```text
Permission Denied
```

---

Actual Cause

```text
AppArmor Profile
```

---

Check

```bash
aa-status
```

---

# Mistake #14

Leaving Unnecessary SUID Binaries

---

Danger

```text
Privilege Escalation
```

---

Audit

```bash
find / -perm -4000
```

---

Visualization

```text
Unused SUID
      │
      ▼
Exploit
      │
      ▼
Root Access
```

---

# Mistake #15

Giving CAP_SYS_ADMIN

---

Most Dangerous Capability

---

Often Called

```text
The New Root
```

---

Visualization

```text
CAP_SYS_ADMIN
      │
      ▼
Massive Privileges
```

---

Use only if absolutely necessary.

---

# Mistake #16

Running Docker With --privileged

---

Command

```bash
docker run --privileged
```

---

Result

```text
Container Nearly Equals Host Root
```

---

Visualization

```text
Container
     │
     ▼
Host Resources
     │
     ▼
Expanded Attack Surface
```

---

# Mistake #17

UID Mismatch in Containers

---

Host

```text
UID 1000
```

---

Container

```text
UID 2000
```

---

Volume Access Fails.

---

Visualization

```text
Container UID
      │
      ▼
Host Ownership
      │
      ▼
Mismatch
      │
      ▼
Denied
```

---

# Mistake #18

Using Shared Directories Without SGID

---

Team Directory

```text
/project
```

---

Users Create Files

Groups become inconsistent.

---

Visualization

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

Fix

```bash
chmod 2775 project
```

---

# Mistake #19

Shared Directory Without Sticky Bit

---

Permissions

```bash
chmod 777 shared
```

---

Users Delete Each Other's Files.

---

Visualization

```text
Alice File
     │
     ▼
Bob Deletes
     │
     ▼
Success
```

---

Fix

```bash
chmod 1777 shared
```

---

# Mistake #20

Not Auditing Permissions

---

Many organizations never check:

```text
World Writable Files

SUID Files

Capabilities

ACLs
```

---

Visualization

```text
Misconfiguration
      │
      ▼
Years Pass
      │
      ▼
Security Incident
```

---

# Real Production Failure #1

Web Server Outage

---

Developer:

```bash
chown -R root /var/www
```

---

Nginx

```text
Cannot Write Cache
```

---

Website Fails.

---

# Real Production Failure #2

Database Backup Leak

---

umask

```text
002
```

---

Backup File

```text
Readable By Other Users
```

---

Sensitive Data Exposure.

---

# Real Production Failure #3

Container Storage Failure

---

Volume Mounted.

---

UID mismatch.

---

Application cannot write.

---

Hours of troubleshooting wasted.

---

# Real Production Failure #4

SELinux Disabled

---

Temporary fix becomes permanent.

---

Months later:

```text
Compromise Occurs
```

---

Protection layer missing.

---

# Prevention Strategy

Always Ask:

```text
Who owns it?

What permissions apply?

Which group applies?

Does ACL exist?

Does SELinux apply?

Does AppArmor apply?

Container?

NFS?

Capability?
```

---

# Professional Workflow

```text
Permission Error
      │
      ▼
Investigate
      │
      ▼
Identify Root Cause
      │
      ▼
Apply Correct Fix
```

---

Never:

```text
Permission Error
      │
      ▼
chmod 777
```

---

# Interview Questions

---

## Why is chmod 777 dangerous?

Everyone receives full access.

---

## Why is CAP_SYS_ADMIN dangerous?

Extremely powerful capability.

---

## Why should SELinux remain enabled?

Additional security layer.

---

## What causes many Docker permission issues?

UID/GID mismatches.

---

## Why use Sticky Bit?

Protect shared directories.

---

## Why use SGID?

Group ownership inheritance.

---

# Quick Checklist

Before changing permissions:

```text
✓ Check Ownership

✓ Check Groups

✓ Check ACLs

✓ Check SELinux

✓ Check AppArmor

✓ Check Mount Options

✓ Check Containers

✓ Check Capabilities
```

---

# Visual Summary

Bad Workflow

```text
Permission Denied
      │
      ▼
chmod 777
      │
      ▼
Security Risk
```

---

Good Workflow

```text
Permission Denied
      │
      ▼
Investigate
      │
      ▼
Root Cause
      │
      ▼
Correct Fix
```

---

# Key Takeaways

1. Most permission problems are configuration problems.

2. chmod 777 is rarely the correct solution.

3. Ownership matters as much as permissions.

4. Directory permissions are often forgotten.

5. ACLs can override expectations.

6. SELinux and AppArmor frequently cause confusion.

7. Containers introduce UID/GID complexity.

8. SUID and capabilities require auditing.

9. Shared directories need SGID and Sticky Bit.

10. Professional administrators troubleshoot systematically.

---

# Next Step

Continue to:

```text
real-world-case-studies.md
```

This file will contain production-grade incidents, outages, security breaches, container permission failures, SELinux troubleshooting stories, NFS disasters, and enterprise lessons learned from real-world Linux environments.
