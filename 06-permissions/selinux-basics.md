# SELinux Basics

> SELinux (Security-Enhanced Linux) is a Mandatory Access Control (MAC) system integrated into the Linux kernel that provides an additional security layer beyond traditional Linux permissions.

Traditional Linux security asks:

```text
"Does the user have permission?"
```

SELinux asks:

```text
"Should this action be allowed at all?"
```

Even if:

```text
Owner permissions allow it
ACLs allow it
Capabilities allow it
Root allows it
```

SELinux can still say:

```text
DENIED
```

This makes SELinux one of the most powerful Linux security technologies ever created.

---

# Learning Objectives

After reading this guide you will understand:

✅ Why SELinux exists

✅ DAC vs MAC

✅ SELinux Architecture

✅ Security Contexts

✅ Labels

✅ Type Enforcement

✅ Policy Engine

✅ Access Vector Cache

✅ SELinux Modes

✅ Process Domains

✅ File Contexts

✅ Container Security

✅ Enterprise Usage

✅ Troubleshooting SELinux

---

# Why SELinux Exists

Before SELinux, Linux primarily relied on:

```text
Permissions
Ownership
ACLs
Capabilities
```

This model is called:

```text
Discretionary Access Control (DAC)
```

---

# DAC Security Problem

Imagine:

```text
Web Server Compromised
```

Attacker gains:

```text
nginx user access
```

---

Nginx has access to:

```text
Web files
Log files
Configuration files
```

---

If nginx is exploited:

```text
Attacker inherits nginx permissions
```

---

Traditional Linux says:

```text
Permissions allow it
```

Access granted.

---

# Real-World Attack Scenario

```text
Attacker
    │
    ▼
Nginx Vulnerability
    │
    ▼
Shell Access
    │
    ▼
Read Sensitive Files
```

DAC alone cannot stop this.

---

# SELinux Solution

SELinux introduces:

```text
Mandatory Access Control
```

Meaning:

```text
Kernel Security Policy
```

decides access.

Not users.

Not applications.

Not file owners.

---

# Security Layer Visualization

Traditional Linux:

```text
User
 │
 ▼
Permissions
 │
 ▼
Kernel
```

---

SELinux:

```text
User
 │
 ▼
Permissions
 │
 ▼
SELinux Policy
 │
 ▼
Kernel
```

Additional protection layer.

---

# DAC vs MAC

---

# DAC

Discretionary Access Control

Controlled by:

```text
Owner
```

Example:

```bash
chmod 777 file.txt
```

Owner decides.

---

# MAC

Mandatory Access Control

Controlled by:

```text
Security Policy
```

Even owner cannot override.

---

# Comparison

| Feature | DAC | MAC |
|----------|----------|----------|
| Controlled By | Owner | Policy |
| chmod Affects | Yes | Limited |
| Root Can Bypass | Usually | No |
| Security Level | Moderate | High |
| Enterprise Usage | Basic | Critical |

---

# SELinux Architecture

SELinux consists of:

```text
Linux Kernel
Policy Engine
Security Contexts
Labels
Access Decisions
```

---

# High-Level Architecture

```text
Application
      │
      ▼
Kernel Request
      │
      ▼
SELinux Policy Engine
      │
      ▼
Allow / Deny
      │
      ▼
Kernel
```

---

# Security Contexts

SELinux identifies everything using:

```text
Security Context
```

Every:

```text
File
Directory
Process
Socket
Port
Device
```

receives a label.

---

# Context Format

```text
user:role:type:level
```

Example:

```text
system_u:object_r:httpd_sys_content_t:s0
```

---

# Context Breakdown

```text
system_u
     │
     ▼
User
```

---

```text
object_r
     │
     ▼
Role
```

---

```text
httpd_sys_content_t
     │
     ▼
Type
```

---

```text
s0
     │
     ▼
Security Level
```

---

# Most Important Component

In real-world SELinux:

```text
TYPE
```

is most important.

---

# Why Types Matter

Example:

```text
httpd_t
```

means:

```text
Apache/Nginx Process
```

---

```text
shadow_t
```

means:

```text
Password File
```

---

SELinux Policy:

```text
httpd_t
CANNOT
read
shadow_t
```

---

Even if:

```text
Root owns file
```

Access denied.

---

# Type Enforcement (TE)

This is the heart of SELinux.

---

# Concept

Every object gets:

```text
Type Label
```

Every process gets:

```text
Domain Label
```

Policy decides:

```text
Domain
     ↓
Can Access?
     ↓
Type
```

---

# Visualization

```text
Process
(httpd_t)
      │
      ▼
File
(shadow_t)
      │
      ▼
Policy Check
      │
      ▼
DENY
```

---

# Example

Apache Process:

```text
httpd_t
```

Attempts:

```text
Read /etc/shadow
```

Label:

```text
shadow_t
```

Result:

```text
DENIED
```

---

# Even Root?

Yes.

Example:

```text
root process
```

running in:

```text
httpd_t
```

domain.

Still denied.

---

# Process Domains

Processes run inside domains.

Example:

```text
sshd_t
```

SSH daemon.

---

```text
httpd_t
```

Web server.

---

```text
mysqld_t
```

Database server.

---

```text
container_t
```

Container process.

---

# View Process Context

```bash
ps -eZ
```

Example:

```text
system_u:system_r:httpd_t:s0
```

---

# View File Context

```bash
ls -Z
```

Example:

```text
httpd_sys_content_t
```

---

# Visual Relationship

```text
Process
(httpd_t)
      │
      ▼
File
(httpd_sys_content_t)
      │
      ▼
Policy Decision
```

---

# Access Vector Cache (AVC)

SELinux decisions are expensive.

Kernel uses:

```text
AVC
```

to cache decisions.

---

# Without AVC

```text
Request
   │
   ▼
Policy Lookup
```

Every time.

Slow.

---

# With AVC

```text
Request
   │
   ▼
AVC Cache
   │
   ▼
Allow / Deny
```

Fast.

---

# AVC Visualization

```text
Process
    │
    ▼
Request
    │
    ▼
AVC Cache
    │
 ┌──┴──┐
 │Hit  │Miss
 ▼     ▼
Allow  Policy Engine
```

---

# SELinux Modes

SELinux operates in three modes.

---

# Enforcing

Strict mode.

```text
Policy Applied

Violations Blocked
```

---

# Permissive

Policy checked.

```text
Violations Logged

NOT Blocked
```

Useful for troubleshooting.

---

# Disabled

SELinux completely off.

Not recommended.

---

# Mode Visualization

```text
Enforcing
     │
     ▼
Block + Log
```

---

```text
Permissive
     │
     ▼
Log Only
```

---

```text
Disabled
     │
     ▼
No Protection
```

---

# View Current Mode

```bash
getenforce
```

Example:

```text
Enforcing
```

---

# Detailed Status

```bash
sestatus
```

---

# Changing Modes

Temporary:

```bash
setenforce 0
```

Permissive.

---

Back:

```bash
setenforce 1
```

Enforcing.

---

# Container Security

Containers heavily rely on SELinux.

Example:

```text
Container A
```

cannot access:

```text
Container B Data
```

even on same host.

---

# OpenShift

One reason OpenShift is highly secure:

```text
SELinux Integration
```

---

# Container Visualization

```text
Container A
(container_t)
```

Cannot access:

```text
Container B Files
(container_t)
```

without policy permission.

---

# Common SELinux File Types

| Type | Purpose |
|---------|---------|
| httpd_t | Web Server |
| sshd_t | SSH Daemon |
| mysqld_t | MySQL |
| shadow_t | Password Files |
| container_t | Containers |
| var_log_t | Logs |
| user_home_t | Home Directories |

---

# Troubleshooting Methodology

Most administrators make a mistake:

```bash
setenforce 0
```

Problem disappears.

Then:

```text
Disable SELinux permanently
```

Bad practice.

---

# Correct Method

Step 1

Check logs:

```bash
ausearch -m avc
```

---

Step 2

Review denials.

---

Step 3

Inspect labels.

```bash
ls -Z
```

---

Step 4

Fix context.

---

# Context Restoration

Restore defaults:

```bash
restorecon -Rv /var/www/html
```

Very common fix.

---

# Common SELinux Problems

---

## Wrong File Context

Most common issue.

Example:

```text
Web file copied manually.
```

Gets wrong label.

---

Apache:

```text
Cannot read file.
```

---

Fix:

```bash
restorecon
```

---

## Incorrect Port Context

Application:

```text
Listening on custom port.
```

SELinux blocks.

---

Need:

```bash
semanage port
```

---

## Disabled SELinux

Security loss.

Avoid.

---

# Enterprise Best Practices

---

## Keep SELinux Enabled

Recommended:

```text
Enforcing Mode
```

---

## Fix Labels

Not policies first.

Most issues:

```text
Wrong Labels
```

---

## Monitor AVC Logs

Regular audits.

---

## Avoid Permanent Disablement

Never:

```bash
SELINUX=disabled
```

without strong justification.

---

# Interview Questions

---

## What is SELinux?

Mandatory Access Control system for Linux.

---

## Difference Between DAC and MAC?

DAC:

```text
Owner Controlled
```

MAC:

```text
Policy Controlled
```

---

## What is Type Enforcement?

Policy based on:

```text
Domain → Type
```

relationships.

---

## What command shows contexts?

```bash
ls -Z
```

---

## What command shows process contexts?

```bash
ps -eZ
```

---

## What is AVC?

Access Vector Cache.

Caches SELinux decisions.

---

## What are SELinux modes?

```text
Enforcing
Permissive
Disabled
```

---

## Why does SELinux deny root?

Policy enforcement overrides traditional permissions.

---

# Quick Cheat Sheet

```bash
getenforce

sestatus

setenforce 0

setenforce 1

ls -Z

ps -eZ

restorecon -Rv /path

ausearch -m avc
```

---

# Visual Summary

Traditional Linux:

```text
User
 │
 ▼
Permissions
 │
 ▼
Access
```

---

SELinux:

```text
User
 │
 ▼
Permissions
 │
 ▼
SELinux Policy
 │
 ▼
Access
```

---

Type Enforcement:

```text
Process
(httpd_t)
     │
     ▼
File
(shadow_t)
     │
     ▼
DENIED
```

---

# Key Takeaways

1. SELinux adds Mandatory Access Control.

2. SELinux enforces policy beyond permissions.

3. Security Contexts label everything.

4. Type Enforcement is the core mechanism.

5. Domains interact with Types.

6. AVC caches security decisions.

7. SELinux can deny even root processes.

8. Containers rely heavily on SELinux.

9. Most SELinux issues are labeling issues.

10. Enterprise Linux systems should run SELinux in Enforcing mode.

---

# Next Step

Continue to:

```text
apparmor-basics.md
```

where you'll learn how AppArmor takes a different approach from SELinux by using **path-based security profiles**, why Ubuntu prefers AppArmor, how profile enforcement works internally, and how AppArmor compares to SELinux in production environments.
