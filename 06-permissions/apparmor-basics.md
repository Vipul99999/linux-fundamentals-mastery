# AppArmor Basics

> AppArmor (Application Armor) is a Linux Security Module (LSM) that protects applications by restricting what files, capabilities, networks, and system resources they can access.

Unlike SELinux, which uses security labels, AppArmor uses:

```text
Path-Based Security
```

This makes AppArmor easier to learn and manage for many administrators.

---

# Learning Objectives

After reading this guide you will understand:

✅ Why AppArmor exists

✅ Linux Security Modules (LSM)

✅ Path-Based Security

✅ AppArmor Architecture

✅ Profiles

✅ Enforce Mode

✅ Complain Mode

✅ Profile Rules

✅ Capability Restrictions

✅ Network Restrictions

✅ Container Security

✅ Kubernetes Integration

✅ AppArmor vs SELinux

✅ Troubleshooting

---

# Why AppArmor Exists

Traditional Linux security relies on:

```text
Permissions

ACLs

Capabilities

Ownership
```

---

Problem:

```text
Application Compromised
```

Attacker gains:

```text
Application Privileges
```

---

Example:

```text
Web Server Exploit
```

Attacker can access:

```text
Everything
the process can access
```

---

Traditional Linux cannot easily limit:

```text
Application Behavior
```

---

# AppArmor Solution

AppArmor asks:

```text
What should this application
be allowed to do?
```

Everything else:

```text
DENIED
```

---

# Security Philosophy

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

AppArmor:

```text
User
 │
 ▼
Permissions
 │
 ▼
AppArmor Profile
 │
 ▼
Access
```

Additional security layer.

---

# Linux Security Modules (LSM)

AppArmor is implemented through:

```text
Linux Security Module
```

Framework.

---

# LSM Architecture

```text
Application
      │
      ▼
System Call
      │
      ▼
Kernel
      │
      ▼
LSM Hook
      │
      ▼
AppArmor
      │
      ▼
Allow / Deny
```

---

# Common Linux Security Modules

```text
SELinux

AppArmor

Smack

TOMOYO

Landlock
```

---

# Core Design Difference

---

# SELinux

Uses:

```text
Labels
```

Example:

```text
httpd_t
shadow_t
```

---

# AppArmor

Uses:

```text
File Paths
```

Example:

```text
/usr/bin/nginx

/etc/nginx/*
```

---

# Visual Comparison

SELinux:

```text
Process Label
      │
      ▼
Object Label
      │
      ▼
Policy
```

---

AppArmor:

```text
Process
      │
      ▼
Path
      │
      ▼
Profile
```

---

# AppArmor Architecture

AppArmor uses:

```text
Profiles
```

A profile describes:

```text
Allowed Actions

Allowed Files

Allowed Capabilities

Allowed Networks
```

---

# High-Level Flow

```text
Application
      │
      ▼
Request Resource
      │
      ▼
Profile Check
      │
      ▼
Allow / Deny
```

---

# Example

Application:

```text
/usr/bin/nginx
```

Profile:

```text
Can Read:
/var/www

Can Read:
/etc/nginx

Cannot Read:
/etc/shadow
```

---

Result

```text
Request:
/etc/shadow

DENIED
```

---

# AppArmor Profiles

Every protected application has:

```text
Profile File
```

---

Typical Location

```text
/etc/apparmor.d/
```

---

Example

```text
/etc/apparmor.d/usr.bin.man
```

```text
/etc/apparmor.d/usr.sbin.nginx
```

```text
/etc/apparmor.d/usr.bin.firefox
```

---

# Profile Visualization

```text
Profile
     │
     ├── Files
     ├── Directories
     ├── Capabilities
     ├── Networking
     └── Signals
```

---

# Viewing Loaded Profiles

```bash
aa-status
```

Example:

```text
50 profiles loaded

45 enforce mode

5 complain mode
```

---

# AppArmor Modes

Every profile operates in a mode.

---

# Enforce Mode

Strict mode.

```text
Violations Blocked
```

and

```text
Logged
```

---

Visualization:

```text
Application
      │
      ▼
Violation
      │
      ▼
DENIED
      │
      ▼
Log Event
```

---

# Complain Mode

Learning mode.

```text
Violations Logged

NOT Blocked
```

Useful for profile development.

---

Visualization:

```text
Application
      │
      ▼
Violation
      │
      ▼
ALLOW
      │
      ▼
Log Event
```

---

# Disable Mode

Profile inactive.

```text
No Protection
```

---

# Mode Comparison

| Mode | Block | Log |
|--------|--------|--------|
| Enforce | Yes | Yes |
| Complain | No | Yes |
| Disabled | No | No |

---

# Check Current Status

```bash
aa-status
```

---

# Switch to Complain Mode

```bash
aa-complain profile
```

Example:

```bash
aa-complain /etc/apparmor.d/usr.sbin.nginx
```

---

# Switch to Enforce Mode

```bash
aa-enforce profile
```

---

# Internal Working

Suppose nginx tries:

```text
Read:
/etc/shadow
```

---

Kernel Flow

```text
System Call
      │
      ▼
LSM Hook
      │
      ▼
AppArmor Profile
      │
      ▼
Path Match?
      │
      ▼
Allow / Deny
```

---

# Visual Internal Flow

```text
Nginx
 │
 ▼
open()
 │
 ▼
Kernel
 │
 ▼
LSM Hook
 │
 ▼
AppArmor
 │
 ▼
Rule Match
 │
 ▼
ALLOW / DENY
```

---

# Basic Profile Syntax

Example:

```text
/etc/nginx/** r,
```

Meaning:

```text
Read Access
```

---

Example:

```text
/var/www/** rw,
```

Meaning:

```text
Read + Write
```

---

Example:

```text
deny /etc/shadow r,
```

Meaning:

```text
Explicit Deny
```

---

# Permission Letters

| Permission | Meaning |
|------------|----------|
| r | Read |
| w | Write |
| m | Memory Map |
| x | Execute |
| k | Lock |
| l | Link |

---

# Capability Restrictions

AppArmor can restrict:

```text
Capabilities
```

Example:

```text
deny capability sys_admin
```

---

Visualization:

```text
Application
     │
     ▼
CAP_SYS_ADMIN
     │
     ▼
DENIED
```

---

# Network Restrictions

AppArmor can control:

```text
TCP

UDP

Raw Sockets
```

---

Example:

```text
network inet stream
```

Allows:

```text
TCP Connections
```

---

# Container Security

Docker uses AppArmor heavily.

---

Default Profile

```text
docker-default
```

---

Container Flow

```text
Container
      │
      ▼
AppArmor Profile
      │
      ▼
Restricted Access
```

---

# Example Restriction

Container attempts:

```text
Mount Filesystem
```

Profile:

```text
DENY
```

Operation blocked.

---

# Kubernetes

Kubernetes supports:

```text
AppArmor Profiles
```

---

Example

```yaml
container.apparmor.security.beta.kubernetes.io/app: localhost/myprofile
```

---

# AppArmor vs SELinux

This is a common interview topic.

---

# Security Model

SELinux:

```text
Label-Based
```

AppArmor:

```text
Path-Based
```

---

# Complexity

SELinux:

```text
More Complex
```

---

AppArmor:

```text
Easier
```

---

# Granularity

SELinux:

```text
Very Fine-Grained
```

---

AppArmor:

```text
Moderate
```

---

# Typical Distributions

SELinux:

```text
RHEL
Rocky
AlmaLinux
Fedora
OpenShift
```

---

AppArmor:

```text
Ubuntu
Debian
openSUSE
```

---

# Visual Comparison

SELinux

```text
Label
 │
 ▼
Policy
 │
 ▼
Access
```

---

AppArmor

```text
Path
 │
 ▼
Profile
 │
 ▼
Access
```

---

# Common Problems

---

# Profile Too Restrictive

Application fails.

Check logs.

---

# Wrong File Path

Application moved.

Profile still references:

```text
Old Path
```

Access denied.

---

# Missing Rules

New feature introduced.

Profile not updated.

---

# Troubleshooting Workflow

Step 1

Check status:

```bash
aa-status
```

---

Step 2

Check logs:

```bash
journalctl -xe
```

---

Step 3

Switch to complain mode:

```bash
aa-complain profile
```

---

Step 4

Observe violations.

---

Step 5

Update profile.

---

Step 6

Return to enforce mode.

---

# Enterprise Best Practices

---

## Use Enforce Mode

Production systems:

```text
Enforce
```

---

## Develop in Complain Mode

Before deployment.

---

## Audit Profiles Regularly

Applications evolve.

Profiles should too.

---

## Restrict Capabilities

Use least privilege.

---

## Protect Critical Services

Examples:

```text
nginx

apache

mysql

postgres

docker
```

---

# Interview Questions

---

## What is AppArmor?

Linux Security Module that uses path-based security profiles.

---

## Difference Between AppArmor and SELinux?

AppArmor:

```text
Path-Based
```

SELinux:

```text
Label-Based
```

---

## What is Enforce Mode?

Violations are blocked.

---

## What is Complain Mode?

Violations logged but not blocked.

---

## Where are profiles stored?

```text
/etc/apparmor.d/
```

---

## What command shows profile status?

```bash
aa-status
```

---

# Quick Cheat Sheet

```bash
aa-status

aa-enforce profile

aa-complain profile

apparmor_parser -r profile

systemctl status apparmor

journalctl -xe
```

---

# Visual Summary

Application Request

```text
Application
      │
      ▼
System Call
      │
      ▼
AppArmor Profile
      │
      ▼
ALLOW / DENY
```

---

Mode Flow

```text
Enforce
    │
    ▼
Block + Log
```

```text
Complain
    │
    ▼
Log Only
```

---

SELinux vs AppArmor

```text
SELinux
(Label Based)
```

```text
Process Label
      │
      ▼
Policy
      │
      ▼
Decision
```

---

```text
AppArmor
(Path Based)
```

```text
File Path
     │
     ▼
Profile
     │
     ▼
Decision
```

---

# Key Takeaways

1. AppArmor is a Linux Security Module.

2. AppArmor uses path-based security.

3. Applications are controlled through profiles.

4. Profiles define allowed actions.

5. Enforce mode blocks violations.

6. Complain mode logs violations.

7. AppArmor is simpler than SELinux.

8. Ubuntu heavily relies on AppArmor.

9. Containers often use AppArmor profiles.

10. AppArmor provides strong application isolation.

---

# Next Step

Continue to:

```text
permission-troubleshooting.md
```

This is where all concepts come together. You'll learn a professional Linux administrator's workflow for diagnosing:

- Permission denied errors
- ACL issues
- SUID/SGID problems
- SELinux denials
- AppArmor blocks
- Capability failures
- Container permission issues
- NFS and Samba permission conflicts

using a systematic troubleshooting methodology.
