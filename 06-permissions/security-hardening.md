# Linux Permission Security Hardening

> Security hardening is the process of reducing security risks by configuring Linux systems according to the Principle of Least Privilege and security best practices.

A default Linux installation is not necessarily a secure Linux installation.

Many successful attacks happen because:

```text
Permissions Too Broad

Unused SUID Binaries

Weak umask Settings

Misconfigured ACLs

Disabled SELinux

Excessive Capabilities

Overprivileged Containers
```

Security hardening reduces the attack surface.

---

# Learning Objectives

After reading this guide you will understand:

✅ Security Hardening Principles

✅ Principle of Least Privilege

✅ Permission Hardening

✅ Ownership Hardening

✅ Group Hardening

✅ umask Security

✅ SUID Auditing

✅ SGID Security

✅ ACL Security

✅ Capability Hardening

✅ SELinux Hardening

✅ AppArmor Hardening

✅ Container Security

✅ Enterprise Best Practices

---

# What Is Security Hardening?

Security hardening means:

```text
Removing Unnecessary Access
```

---

# Security Philosophy

Bad Security:

```text
Allow Everything

Block Nothing
```

---

Good Security:

```text
Allow Only What Is Needed

Deny Everything Else
```

---

# Security Pyramid

```text
+---------------------+
| Monitoring          |
+---------------------+
| SELinux/AppArmor    |
+---------------------+
| ACLs                |
+---------------------+
| Permissions         |
+---------------------+
| Ownership           |
+---------------------+
```

Each layer strengthens security.

---

# Principle of Least Privilege

Most important security principle.

---

# Definition

Give:

```text
Minimum Permissions
```

Required to perform a task.

Nothing more.

---

# Example

Bad:

```bash
chmod 777 project
```

Everyone gets:

```text
Read

Write

Execute
```

---

Good:

```bash
chmod 750 project
```

Only authorized users.

---

# Visualization

Bad:

```text
Everyone
   │
   ▼
Full Access
```

---

Good:

```text
Authorized User
      │
      ▼
Required Access
```

---

# Permission Hardening

---

# Avoid 777

One of the most common mistakes.

---

Bad:

```bash
chmod 777 file
```

---

Why Dangerous?

Anyone can:

```text
Read

Modify

Delete

Replace
```

---

# Secure Alternatives

Files:

```text
644
640
600
```

---

Directories:

```text
755
750
700
```

---

# Recommended File Permissions

| File Type | Permission |
|------------|------------|
| Public Config | 644 |
| Internal Config | 640 |
| Secret Config | 600 |
| SSH Keys | 600 |
| Scripts | 750 |
| Private Scripts | 700 |

---

# Ownership Hardening

Ownership controls access evaluation.

---

# Principle

Files should be owned by:

```text
Correct User

Correct Group
```

---

Bad Example

```text
Application Files

Owner: root
```

Application:

```text
Cannot Write Logs
```

---

Another Bad Example

```text
Sensitive Files

Owner: developer
```

Should belong to:

```text
Service Account
```

---

# Ownership Flow

```text
Resource
     │
     ▼
Correct Owner?
     │
   YES
     │
     ▼
Permission Evaluation
```

---

# Group Hardening

Use groups.

Avoid:

```text
Per-user permissions
```

---

Good Design

```text
developers

finance

security

database
```

---

Visualization

```text
Users
   │
   ▼
Groups
   │
   ▼
Permissions
```

---

# Secure umask

Default file creation is critical.

---

Bad:

```bash
umask 000
```

Creates:

```text
World Accessible Files
```

---

Good:

```bash
umask 027
```

---

High Security

```bash
umask 077
```

---

Visualization

```text
Create File
      │
      ▼
Apply umask
      │
      ▼
Secure Defaults
```

---

# SUID Hardening

SUID is powerful and dangerous.

---

# Security Risk

Compromised SUID binary:

```text
Privilege Escalation
```

---

Audit Regularly

```bash
find / -perm -4000 -type f
```

---

Visualization

```text
SUID Binary
      │
      ▼
Exploit
      │
      ▼
Root Access
```

---

# Hardening Strategy

```text
Remove Unused SUID

Patch Regularly

Monitor Changes
```

---

# SGID Hardening

SGID useful for:

```text
Shared Directories
```

---

Risk:

```text
Sensitive Group Exposure
```

---

Audit:

```bash
find / -perm -2000
```

---

# Sticky Bit Hardening

Shared directories:

```text
/tmp

/var/tmp

shared workspaces
```

---

Verify:

```bash
ls -ld /tmp
```

Expected:

```text
drwxrwxrwt
```

---

# ACL Hardening

ACLs can become:

```text
Complex

Hidden

Difficult to Audit
```

---

Audit:

```bash
getfacl -R directory
```

---

Visualization

```text
ACL Entry
      │
      ▼
Unexpected Access
```

---

# Capability Hardening

Capabilities should replace:

```text
SUID
```

when possible.

---

Bad

```text
Full Root Access
```

---

Better

```text
Specific Capability
```

---

Audit

```bash
getcap -r /
```

---

Dangerous Capability

```text
CAP_SYS_ADMIN
```

Often called:

```text
New Root
```

---

# SELinux Hardening

Most important enterprise protection.

---

Bad

```bash
setenforce 0
```

Permanent disablement.

---

Good

```text
Enforcing Mode
```

---

Verify

```bash
getenforce
```

Expected:

```text
Enforcing
```

---

# SELinux Security Flow

```text
Application
      │
      ▼
Permission Check
      │
      ▼
SELinux Policy
      │
      ▼
Allow / Deny
```

---

# AppArmor Hardening

Ubuntu environments.

---

Production Recommendation

```text
Enforce Mode
```

---

Check

```bash
aa-status
```

---

Visualization

```text
Application
      │
      ▼
Profile
      │
      ▼
Restricted Access
```

---

# Service Account Hardening

Never run applications as root.

---

Bad

```text
nginx → root
```

---

Good

```text
nginx → nginx

mysql → mysql

postgres → postgres
```

---

Visualization

```text
Application
      │
      ▼
Dedicated Account
      │
      ▼
Limited Damage
```

---

# Log Protection

Logs contain:

```text
Passwords

Tokens

Session IDs

IP Addresses
```

---

Recommended

```bash
chmod 640 logs
```

---

# SSH Hardening

---

SSH Directory

```bash
chmod 700 ~/.ssh
```

---

Private Key

```bash
chmod 600 ~/.ssh/id_rsa
```

---

Public Key

```bash
chmod 644 ~/.ssh/id_rsa.pub
```

---

Visualization

```text
SSH Key
     │
     ▼
Incorrect Permission
     │
     ▼
SSH Refuses Login
```

---

# Container Hardening

Containers often run with excessive permissions.

---

Bad

```bash
docker run --privileged
```

---

Why Dangerous?

Container receives:

```text
Almost Full Host Access
```

---

Better

```bash
docker run --cap-drop ALL
```

Then:

```text
Add Only Required Capabilities
```

---

Container Security Layers

```text
Container
      │
      ▼
Capabilities
      │
      ▼
AppArmor/SELinux
      │
      ▼
Kernel
```

---

# Kubernetes Hardening

Avoid:

```yaml
privileged: true
```

---

Prefer:

```yaml
runAsNonRoot: true
```

---

Example

```yaml
securityContext:
  runAsNonRoot: true
```

---

# World Writable Audit

Find dangerous files:

```bash
find / -perm -002 -type f
```

---

Visualization

```text
World Writable File
        │
        ▼
Attacker Modification
```

---

# Security Auditing Checklist

Ownership

```bash
find / -nouser
```

---

World Writable

```bash
find / -perm -002
```

---

SUID

```bash
find / -perm -4000
```

---

SGID

```bash
find / -perm -2000
```

---

Capabilities

```bash
getcap -r /
```

---

SELinux

```bash
sestatus
```

---

AppArmor

```bash
aa-status
```

---

# Enterprise Security Workflow

```text
Asset
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
SELinux/AppArmor
  │
  ▼
Monitoring
```

---

# Common Hardening Mistakes

---

## Using 777

Still the most common.

---

## Disabling SELinux

Security reduction.

---

## Running Services as Root

High risk.

---

## Excessive ACLs

Difficult to audit.

---

## Ignoring SUID Binaries

Privilege escalation risk.

---

## Using --privileged Containers

Massive attack surface.

---

# Real Attack Scenario

Misconfigured Web Directory

Permissions:

```text
777
```

---

Attacker Uploads:

```text
Web Shell
```

---

Executes Commands:

```text
Remote Code Execution
```

---

Hardening Prevents:

```text
Unauthorized Modification
```

---

# Interview Questions

---

## What is the Principle of Least Privilege?

Grant only required access.

---

## Why is 777 dangerous?

Everyone gains full access.

---

## Why audit SUID binaries?

Privilege escalation risk.

---

## Why keep SELinux enabled?

Additional protection layer.

---

## Why avoid privileged containers?

Near-root access.

---

## What permission should SSH private keys use?

```text
600
```

---

## Which capability is most dangerous?

```text
CAP_SYS_ADMIN
```

---

# Quick Hardening Checklist

```text
✓ Use Least Privilege

✓ Avoid 777

✓ Secure umask

✓ Audit SUID

✓ Audit SGID

✓ Audit ACLs

✓ Minimize Capabilities

✓ Keep SELinux Enabled

✓ Keep AppArmor Enabled

✓ Run Services as Dedicated Users

✓ Harden Containers

✓ Audit Regularly
```

---

# Visual Summary

Secure System

```text
User
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
SELinux/AppArmor
 │
 ▼
Monitoring
 │
 ▼
Protected System
```

---

# Key Takeaways

1. Security hardening reduces attack surface.

2. Least Privilege is the foundation.

3. Avoid world-writable permissions.

4. Secure ownership matters.

5. SUID binaries require auditing.

6. Capabilities are safer than SUID.

7. SELinux and AppArmor should remain enabled.

8. Containers require additional hardening.

9. Security is layered, not a single setting.

10. Regular auditing is essential.

---

# Next Step

Continue to:

```text
common-mistakes.md
```

This file will cover dozens of real-world permission mistakes made by Linux beginners, administrators, DevOps engineers, Docker users, Kubernetes operators, and security teams, along with their consequences and correct solutions.
