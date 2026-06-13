# References & Further Reading

> This document contains official documentation, books, standards, tools, and learning resources related to Linux permissions, access control, security hardening, SELinux, AppArmor, capabilities, and enterprise Linux administration.

Use this file as:

```text
Learning Roadmap

Research Hub

Professional Reference Library
```

---

# Learning Path

Recommended learning order:

```text
Ownership
      │
      ▼
Permissions
      │
      ▼
Groups
      │
      ▼
umask
      │
      ▼
SUID / SGID
      │
      ▼
Sticky Bit
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
Troubleshooting
      │
      ▼
Security Hardening
```

---

# Official Linux Documentation

---

## Linux Manual Pages

The most important source.

Examples:

```bash
man chmod
man chown
man chgrp
man umask
man setfacl
man getfacl
man setcap
man getcap
```

---

Why Important?

```text
Always Available

Distribution Independent

Authoritative
```

---

# GNU Coreutils Documentation

Covers:

```text
chmod

chown

chgrp
```

Topics:

```text
Permission Management

Ownership

File Utilities
```

---

# Linux Kernel Documentation

The kernel is the ultimate source of truth.

Topics:

```text
Capabilities

Security Modules

Namespaces

Filesystem Permissions
```

---

Recommended Areas

```text
Security

Filesystems

LSM

Credentials
```

---

# POSIX Standards

Linux permissions originate from POSIX standards.

Topics:

```text
File Permissions

User IDs

Group IDs

Access Control
```

---

Why Learn?

```text
Understand why Linux behaves as it does.
```

---

# Security Documentation

---

# SELinux Documentation

Topics:

```text
Type Enforcement

Policies

Contexts

AVC

MLS

MCS
```

---

Recommended For

```text
RHEL

Rocky Linux

AlmaLinux

Fedora

OpenShift
```

---

# AppArmor Documentation

Topics:

```text
Profiles

Modes

Path Restrictions

Container Security
```

---

Recommended For

```text
Ubuntu

Debian

openSUSE
```

---

# Linux Capabilities Documentation

Topics:

```text
Capability Sets

Effective

Permitted

Bounding

Ambient
```

---

Must Learn

```text
CAP_SYS_ADMIN

CAP_NET_BIND_SERVICE

CAP_NET_RAW
```

---

# Enterprise Security Standards

---

# CIS Benchmarks

CIS = Center for Internet Security

Topics:

```text
Permissions

Ownership

SUID Auditing

SELinux

SSH Hardening
```

---

Why Important?

```text
Used by Enterprises Worldwide.
```

---

# NIST Security Guidance

Topics:

```text
Access Control

Least Privilege

System Hardening

Risk Management
```

---

Recommended For

```text
Security Engineers

Compliance Teams
```

---

# Industry Frameworks

Useful concepts:

```text
Least Privilege

Defense in Depth

Zero Trust

Security Monitoring
```

---

# Books

---

## Linux Basics for Hackers

Topics:

```text
Permissions

Ownership

Linux Security
```

---

Good For

```text
Beginners
```

---

## How Linux Works

Topics:

```text
Processes

Permissions

Kernel Internals

Filesystems
```

---

Good For

```text
Intermediate Learners
```

---

## Linux System Programming

Topics:

```text
UID

GID

Permissions

System Calls
```

---

Good For

```text
Advanced Learners
```

---

## Linux Kernel Development

Topics:

```text
Credentials

Capabilities

Kernel Security
```

---

Good For

```text
Kernel Enthusiasts
```

---

# Security Books

---

## SELinux System Administration

Topics:

```text
Policy Writing

Contexts

Enterprise Security
```

---

## Practical Linux Security Cookbook

Topics:

```text
Permissions

Hardening

Auditing

Monitoring
```

---

# Red Hat Resources

Important For:

```text
RHCSA

RHCE

RHEL Administration
```

---

Topics:

```text
SELinux

Permissions

ACLs

Security
```

---

# Ubuntu Resources

Important For:

```text
Ubuntu Administration

AppArmor
```

---

Topics:

```text
AppArmor

Permissions

Security
```

---

# Container Security Resources

Topics:

```text
Docker Security

Kubernetes Security

Capabilities

AppArmor

SELinux
```

---

Must Learn

```text
UID/GID Mapping

Volumes

Namespaces

Security Contexts
```

---

# Cloud Security Resources

Topics:

```text
Linux in Cloud

Least Privilege

IAM vs Linux Permissions

Containers
```

---

Relevant Platforms

```text
AWS

Azure

Google Cloud
```

---

# Linux Security Tools

---

## Ownership & Permissions

Commands:

```bash
ls -l

chmod

chown

chgrp
```

---

## ACL

Commands:

```bash
getfacl

setfacl
```

---

## Capabilities

Commands:

```bash
getcap

setcap

capsh
```

---

## SELinux

Commands:

```bash
ls -Z

sestatus

getenforce

restorecon

ausearch
```

---

## AppArmor

Commands:

```bash
aa-status

aa-enforce

aa-complain
```

---

# Auditing Tools

Useful Commands

---

Find SUID

```bash
find / -perm -4000
```

---

Find SGID

```bash
find / -perm -2000
```

---

Find Sticky Bit

```bash
find / -perm -1000
```

---

Find World Writable

```bash
find / -perm -002
```

---

Find Capabilities

```bash
getcap -r /
```

---

# Filesystem References

Understand:

```text
Inodes

Metadata

Ownership

Permission Storage
```

---

Visualization

```text
File
 │
 ▼
Inode
 │
 ├── Owner
 ├── Group
 ├── Permissions
 ├── ACL
 └── Metadata
```

---

# Security Concepts To Master

---

## Principle of Least Privilege

Grant:

```text
Only Required Access
```

---

## Defense in Depth

Multiple security layers.

Visualization:

```text
Permissions

ACL

Capabilities

SELinux

AppArmor
```

---

## Separation of Duties

Example:

```text
Developers

Admins

Security Team
```

Different access levels.

---

## Zero Trust

Never assume:

```text
Access Should Be Allowed
```

Verify everything.

---

# Practice Resources

Build Labs For:

```text
chmod

ACL

SUID

SGID

Sticky Bit

SELinux

Containers
```

---

Recommended Environments

```text
Virtual Machines

Docker Containers

Cloud Instances

Home Lab
```

---

# Home Lab Ideas

---

## Shared Project Directory

Practice:

```text
Groups

SGID

ACL
```

---

## Secure Web Server

Practice:

```text
Ownership

Permissions

SELinux
```

---

## Multi-User Environment

Practice:

```text
Sticky Bit

ACL

Auditing
```

---

## Container Storage

Practice:

```text
UID

GID

Volume Permissions
```

---

# Certification Mapping

---

## RHCSA

Important Topics

```text
Permissions

Ownership

ACL

SELinux
```

---

## RHCE

Adds:

```text
Advanced SELinux

Automation
```

---

## Linux+

Topics:

```text
Permissions

Security

Users

Groups
```

---

## LPIC

Topics:

```text
Ownership

ACL

Security
```

---

# DevOps Mapping

Must Know

```text
Permissions

Containers

Capabilities

SELinux

Troubleshooting
```

---

# Security Engineer Mapping

Must Know

```text
Least Privilege

SUID

Capabilities

SELinux

Auditing
```

---

# SRE Mapping

Must Know

```text
Permission Troubleshooting

Containers

Incident Response
```

---

# Interview Preparation Checklist

Before Interviews:

```text
✓ Understand Ownership

✓ Understand chmod

✓ Understand ACL

✓ Understand SUID

✓ Understand SGID

✓ Understand Sticky Bit

✓ Understand Capabilities

✓ Understand SELinux

✓ Understand AppArmor

✓ Understand Troubleshooting
```

---

# Mastery Checklist

You understand Linux permissions when you can:

```text
✓ Explain Ownership

✓ Explain Permission Evaluation

✓ Use chmod Without Memorization

✓ Troubleshoot Permission Denied

✓ Configure ACLs

✓ Audit SUID

✓ Use Capabilities

✓ Diagnose SELinux Issues

✓ Diagnose AppArmor Issues

✓ Design Secure Access Models
```

---

# Visual Learning Map

```text
Users
  │
  ▼
Groups
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
Security Hardening
  │
  ▼
Production Systems
```

---

# Final Takeaways

1. Linux permissions are foundational to system administration.

2. Ownership and permissions are only the beginning.

3. ACLs extend traditional access control.

4. Capabilities replace many root requirements.

5. SELinux and AppArmor provide mandatory access control.

6. Security hardening requires multiple layers.

7. Troubleshooting skills are more valuable than memorization.

8. Real-world systems combine all concepts together.

9. Continuous practice is essential.

10. Mastering permissions makes you a stronger Linux administrator, DevOps engineer, SRE, and security professional.

---

# End of 06-Permissions

Next Module:

```text
07-process-management/
```

Where you'll learn:

```text
Processes

PID

PPID

Foreground & Background Jobs

Signals

Scheduling

Systemd

cgroups

Namespaces

Process Monitoring

Performance Tuning

Process Security
```
