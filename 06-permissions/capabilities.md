# Linux Capabilities

> Linux Capabilities divide the enormous power of the root user into smaller, independent privileges.

Instead of:

```text
Root = Everything
```

Linux Capabilities allow:

```text
Give only the privilege required
```

This follows the most important security principle:

```text
Principle of Least Privilege
```

Capabilities are one of the biggest security improvements ever added to Linux.

---

# Learning Objectives

After reading this guide you will understand:

✅ Why Capabilities exist

✅ Limitations of Root

✅ Problems with SUID

✅ Capability Architecture

✅ Kernel Capability Model

✅ File Capabilities

✅ Process Capabilities

✅ Capability Sets

✅ Effective, Permitted, Inheritable Sets

✅ Bounding Set

✅ Ambient Set

✅ Docker Capabilities

✅ Kubernetes Security

✅ Capability Hardening

✅ Enterprise Best Practices

---

# The Traditional Linux Model

Historically Linux had only two privilege levels.

```text
Normal User

Root User
```

---

# Visual Representation

```text
User
 │
 ▼
Limited Access
```

---

```text
Root
 │
 ▼
Unlimited Access
```

---

# The Problem

Suppose a program only needs:

```text
Bind to Port 80
```

Traditionally:

```text
Run as Root
```

---

# Security Issue

Root receives:

```text
Network Control

Filesystem Control

User Management

Kernel Control

Everything
```

even though:

```text
Only Port 80
```

was required.

---

# Example

Web Server:

```text
nginx
```

Needs:

```text
Port 80
```

Traditionally:

```text
Root Required
```

Dangerous.

---

# Linux Capability Solution

Instead:

```text
Give only:

CAP_NET_BIND_SERVICE
```

Now:

```text
Program binds to port 80

Without full root privileges.
```

---

# Evolution of Linux Security

Old Model:

```text
Root
 ├── All Privileges
 ├── All Devices
 ├── All Files
 ├── All Networking
 └── Everything
```

---

New Model:

```text
Root Privileges
      │
      ▼
Split Into
      │
      ▼
Capabilities
```

---

# Capability Architecture

```text
Root Power
     │
     ▼
+--------------------+
| CAP_NET_ADMIN      |
| CAP_NET_RAW        |
| CAP_SYS_ADMIN      |
| CAP_SYS_TIME       |
| CAP_SYS_BOOT       |
| CAP_CHOWN          |
| CAP_SETUID         |
| CAP_SETGID         |
| CAP_KILL           |
| CAP_MKNOD          |
+--------------------+
```

Each capability controls one area.

---

# Internal Kernel Design

Kernel stores capability information inside:

```text
task_struct
```

(Process Descriptor)

---

# Process Structure

```text
Process
 │
 ▼
task_struct
 │
 ├── PID
 ├── UID
 ├── GID
 ├── Memory Info
 └── Capability Sets
```

---

# Capability Check Flow

Example:

```text
Program wants:
Change System Time
```

Kernel:

```text
Has CAP_SYS_TIME?
```

---

If:

```text
YES
```

Allow.

---

If:

```text
NO
```

Deny.

---

# Visual Flow

```text
Application
      │
      ▼
Kernel Request
      │
      ▼
Capability Check
      │
   YES / NO
      │
      ▼
ALLOW / DENY
```

---

# Listing Available Capabilities

Modern kernels support dozens of capabilities.

Common examples:

---

# CAP_CHOWN

Allows:

```text
Change file ownership
```

Equivalent to:

```bash
chown
```

operations.

---

# CAP_DAC_OVERRIDE

Bypass:

```text
File Permission Checks
```

Very powerful.

---

# CAP_NET_BIND_SERVICE

Allows:

```text
Bind to ports below 1024
```

Example:

```text
80
443
22
```

---

# CAP_NET_ADMIN

Allows:

```text
Interface Configuration

Routing Changes

Firewall Management
```

---

# CAP_SYS_TIME

Allows:

```text
Change system clock
```

---

# CAP_SYS_BOOT

Allows:

```text
Reboot system
```

---

# CAP_KILL

Allows:

```text
Send signals
to arbitrary processes
```

---

# CAP_SETUID

Allows:

```text
Manipulate User IDs
```

---

# CAP_SETGID

Allows:

```text
Manipulate Group IDs
```

---

# Why Capabilities Are Better Than SUID

---

# SUID

Example:

```text
Program
```

gets:

```text
Entire Root Privilege Set
```

---

# Capabilities

Program receives:

```text
Only Needed Privileges
```

---

# Visual Comparison

SUID:

```text
Program
 │
 ▼
ROOT
 │
 ▼
Everything
```

---

Capabilities:

```text
Program
 │
 ▼
CAP_NET_BIND_SERVICE
 │
 ▼
Only Port Binding
```

---

# File Capabilities

Capabilities can be attached directly to files.

Similar to:

```text
SUID
```

but safer.

---

# Viewing File Capabilities

Command:

```bash
getcap /path/to/file
```

Example:

```bash
getcap /usr/bin/ping
```

Output:

```text
cap_net_raw=ep
```

---

# Why ping Uses Capabilities

Ping sends:

```text
Raw Network Packets
```

Requires:

```text
CAP_NET_RAW
```

---

Instead of:

```text
SUID Root
```

Modern systems use:

```text
Capability
```

---

# Assigning File Capabilities

Command:

```bash
setcap
```

Example:

```bash
setcap cap_net_bind_service=+ep myserver
```

Now:

```text
Can bind port 80
```

without root.

---

# Verify

```bash
getcap myserver
```

Output:

```text
cap_net_bind_service=ep
```

---

# Understanding Capability Sets

This is where Linux gets advanced.

Every process has multiple capability sets.

---

# Five Important Sets

```text
Permitted

Effective

Inheritable

Bounding

Ambient
```

---

# Capability Set Visualization

```text
Process
 │
 ├── Permitted
 ├── Effective
 ├── Inheritable
 ├── Bounding
 └── Ambient
```

---

# Permitted Set

Maximum capabilities process may use.

Think:

```text
Capability Inventory
```

---

# Effective Set

Capabilities currently active.

Kernel checks this set.

---

# Inheritable Set

Capabilities passed:

```text
Parent → Child
```

during execution.

---

# Bounding Set

Upper security limit.

Even root cannot exceed it.

---

# Ambient Set

Capabilities preserved across:

```text
execve()
```

calls.

Useful in containers.

---

# Capability Flow

```text
File Capability
        │
        ▼
Permitted Set
        │
        ▼
Effective Set
        │
        ▼
Kernel Check
```

---

# Viewing Process Capabilities

Check current shell:

```bash
grep Cap /proc/self/status
```

Output:

```text
CapInh
CapPrm
CapEff
CapBnd
CapAmb
```

---

# Decoding Capabilities

Install:

```bash
capsh
```

Example:

```bash
capsh --print
```

Shows human-readable capabilities.

---

# Capabilities in Docker

Containers heavily use capabilities.

---

# Traditional Container

Without restrictions:

```text
Almost Root
```

Dangerous.

---

# Docker Removes Many Capabilities

Default Docker container:

```text
Drops dozens
of capabilities
```

for security.

---

# View Container Capabilities

```bash
docker inspect container
```

or

```bash
capsh --print
```

inside container.

---

# Adding Capability

```bash
docker run \
--cap-add NET_ADMIN
```

---

# Removing Capability

```bash
docker run \
--cap-drop ALL
```

---

# Kubernetes Security

Pods can use:

```yaml
securityContext:
  capabilities:
    add:
      - NET_ADMIN
```

---

Or:

```yaml
drop:
  - ALL
```

Recommended.

---

# Enterprise Hardening

---

# Principle of Least Privilege

Only grant:

```text
Required Capabilities
```

Never more.

---

# Replace SUID Programs

Prefer:

```text
Capabilities
```

where possible.

---

# Audit Regularly

Find capabilities:

```bash
getcap -r /
```

---

# Minimize Container Privileges

Use:

```yaml
drop:
  - ALL
```

then add only required capabilities.

---

# Common Mistakes

---

## Giving CAP_SYS_ADMIN

Often called:

```text
"The New Root"
```

Very powerful.

Avoid when possible.

---

## Excessive Container Privileges

Bad:

```bash
--privileged
```

Equivalent to:

```text
Almost Full Root
```

---

## Forgetting Bounding Set

Capabilities may appear present but still fail.

Bounding Set blocks them.

---

# Troubleshooting

---

# Capability Not Working

Check:

```bash
getcap binary
```

---

# Process Still Denied

Check:

```bash
capsh --print
```

---

# Container Permission Issues

Inspect:

```bash
docker inspect
```

Capabilities.

---

# Capability Lost After Execution

Likely:

```text
Inheritable
Ambient
```

issue.

---

# Auditing Capabilities

Find all binaries:

```bash
getcap -r / 2>/dev/null
```

---

# Example Output

```text
/usr/bin/ping
/usr/bin/mtr
```

---

# Interview Questions

---

## Why were Capabilities introduced?

To break root privileges into smaller pieces.

---

## What problem do Capabilities solve?

Least Privilege.

---

## Difference between SUID and Capabilities?

SUID grants:

```text
Full owner privileges.
```

Capabilities grant:

```text
Specific privileges.
```

---

## Which capability allows binding port 80?

```text
CAP_NET_BIND_SERVICE
```

---

## Which capability allows raw packets?

```text
CAP_NET_RAW
```

---

## Which capability is considered most dangerous?

```text
CAP_SYS_ADMIN
```

---

## How do you view file capabilities?

```bash
getcap file
```

---

## How do you assign capabilities?

```bash
setcap
```

---

# Quick Cheat Sheet

```bash
getcap file

getcap -r /

setcap cap_net_bind_service=+ep app

setcap cap_net_raw=+ep app

capsh --print

grep Cap /proc/self/status
```

---

# Visual Summary

Old Model:

```text
User
 │
 ▼
Root
 │
 ▼
Everything
```

---

Modern Model:

```text
User
 │
 ▼
Capability
 │
 ▼
Specific Privilege
```

---

Kernel Flow:

```text
Program
   │
   ▼
Capability Request
   │
   ▼
Effective Set
   │
   ▼
Kernel Check
   │
   ▼
ALLOW / DENY
```

---

# Key Takeaways

1. Capabilities divide root privileges.

2. They implement least privilege.

3. They are safer than SUID.

4. File capabilities can replace many SUID binaries.

5. Every process has capability sets.

6. Effective Set is what the kernel checks.

7. Containers rely heavily on capabilities.

8. Kubernetes exposes capability controls.

9. CAP_SYS_ADMIN is extremely powerful.

10. Capabilities are fundamental to modern Linux security.

---

# Next Step

Continue to:

```text
selinux-basics.md
```

where you'll learn why Linux permissions, ACLs, SUID, SGID, and Capabilities are still not enough for high-security systems, and how SELinux introduces **Mandatory Access Control (MAC)** to enforce security policies even against root-owned processes.
