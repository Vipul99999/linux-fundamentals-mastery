# Linux Permissions Interview Questions (Part 2)

> Intermediate to Advanced Linux Permission, Security, Troubleshooting, and Production Interview Questions.

This guide focuses on:

```text
Real-world troubleshooting
Production incidents
Security implications
ACLs
SUID/SGID
Capabilities
SELinux
AppArmor
Containers
Best practices
```

---

# Section 1
# Permission Troubleshooting

---

## Q1.

A user reports:

```text
Permission Denied
```

What is your troubleshooting workflow?

### Answer

Follow this order:

```text
1. File exists?
2. Ownership
3. Permissions
4. Parent directory permissions
5. Group membership
6. ACLs
7. Mount options
8. Capabilities
9. SELinux
10. AppArmor
11. Container/NFS issues
```

---

## Q2.

A file has:

```text
777
```

permissions but access is still denied.

What could be wrong?

### Answer

Possible causes:

```text
SELinux

AppArmor

Read-only filesystem

noexec mount

ACL restrictions

Container isolation
```

---

## Q3.

How do you determine whether ACLs exist?

### Answer

```bash
ls -l
```

Look for:

```text
+
```

Example:

```text
-rw-r-----+
```

Then:

```bash
getfacl file
```

---

## Q4.

How would you investigate a directory access problem?

### Answer

Check:

```bash
namei -l /path/to/file
```

This shows permissions for every directory in the path.

---

## Q5.

A user can read a file but cannot modify it.

What should you check?

### Answer

```text
Write permission

ACLs

Read-only mount

SELinux/AppArmor
```

---

# Section 2
# Ownership & Groups

---

## Q6.

Why should permissions be assigned through groups rather than individual users?

### Answer

Benefits:

```text
Scalable

Easier administration

Simpler auditing

Consistent access control
```

---

## Q7.

What happens if a user belongs to multiple groups?

### Answer

Linux evaluates:

```text
All group memberships
```

If any applicable group grants access, access may be allowed.

---

## Q8.

How can you see all groups a user belongs to?

### Answer

```bash
id username
```

or

```bash
groups username
```

---

## Q9.

What is a service account?

### Answer

Dedicated account used by:

```text
nginx

mysql

postgres

redis

docker
```

instead of root.

---

## Q10.

Why should services not run as root?

### Answer

Limits damage if compromised.

Implements:

```text
Least Privilege
```

---

# Section 3
# ACL Interview Questions

---

## Q11.

Why were ACLs introduced?

### Answer

Traditional permissions only support:

```text
Owner

Group

Others
```

ACLs allow:

```text
Multiple users

Multiple groups
```

without changing ownership.

---

## Q12.

How do ACLs affect performance?

### Answer

Minimal impact in modern systems.

The management complexity is usually a bigger concern.

---

## Q13.

What is a default ACL?

### Answer

Permissions automatically inherited by newly created files/directories.

Example:

```bash
setfacl -d
```

---

## Q14.

Can ACLs override traditional permissions?

### Answer

Yes.

Example:

```text
640 file
```

but ACL grants another user access.

---

## Q15.

What is the most misunderstood ACL feature?

### Answer

ACL Mask.

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

---

# Section 4
# SUID / SGID / Sticky Bit

---

## Q16.

What security risks do SUID programs create?

### Answer

Potential:

```text
Privilege Escalation

Command Injection

Buffer Overflow Exploitation
```

---

## Q17.

Why is SUID considered dangerous?

### Answer

Because code executes with elevated privileges.

A vulnerability can lead to:

```text
Root Access
```

---

## Q18.

How do you audit SUID binaries?

### Answer

```bash
find / -perm -4000 -type f
```

---

## Q19.

When should SGID be used?

### Answer

Shared project directories.

---

Example:

```text
Developers

Finance

Operations
```

---

## Q20.

Why is Sticky Bit important?

### Answer

Protects files in shared writable directories.

Example:

```text
/tmp
```

---

# Section 5
# Capabilities

---

## Q21.

Why were capabilities introduced?

### Answer

To split root privileges into smaller pieces.

---

Traditional model:

```text
Root = Everything
```

Modern model:

```text
Specific Privilege
```

---

## Q22.

What capability allows binding to port 80?

### Answer

```text
CAP_NET_BIND_SERVICE
```

---

## Q23.

Which capability is considered extremely dangerous?

### Answer

```text
CAP_SYS_ADMIN
```

Often called:

```text
The New Root
```

---

## Q24.

How do capabilities improve security?

### Answer

Grant only required privileges.

Reduces attack surface.

---

## Q25.

How do you view capabilities on a binary?

### Answer

```bash
getcap binary
```

---

# Section 6
# SELinux

---

## Q26.

A website returns 403.

Permissions are correct.

What should you investigate next?

### Answer

SELinux context.

```bash
ls -Z
```

---

## Q27.

What is the most common SELinux issue?

### Answer

Incorrect file labels.

---

## Q28.

What command restores default labels?

### Answer

```bash
restorecon
```

---

## Q29.

Why should SELinux remain enabled?

### Answer

Provides:

```text
Mandatory Access Control
```

even if permissions are misconfigured.

---

## Q30.

Difference between:

```text
Enforcing

Permissive

Disabled
```

### Answer

```text
Enforcing  -> Block + Log

Permissive -> Log Only

Disabled   -> No Protection
```

---

# Section 7
# AppArmor

---

## Q31.

What is AppArmor?

### Answer

Path-based Mandatory Access Control.

---

## Q32.

How does AppArmor differ from SELinux?

### Answer

SELinux:

```text
Label Based
```

AppArmor:

```text
Path Based
```

---

## Q33.

What is complain mode?

### Answer

Violations:

```text
Logged

Not Blocked
```

---

## Q34.

How do you view loaded profiles?

### Answer

```bash
aa-status
```

---

## Q35.

Where are AppArmor profiles stored?

### Answer

```text
/etc/apparmor.d/
```

---

# Section 8
# Security Hardening

---

## Q36.

What is the Principle of Least Privilege?

### Answer

Grant:

```text
Only Required Access
```

Nothing more.

---

## Q37.

Why is chmod 777 considered bad practice?

### Answer

Anyone can:

```text
Read

Modify

Execute
```

the resource.

---

## Q38.

What is Defense in Depth?

### Answer

Multiple security layers.

Example:

```text
Permissions

ACL

Capabilities

SELinux

AppArmor
```

---

## Q39.

Why should world-writable files be audited?

### Answer

Attackers can:

```text
Modify

Replace

Delete
```

data.

---

## Q40.

How do you find world-writable files?

### Answer

```bash
find / -perm -002
```

---

# Section 9
# Containers & Cloud

---

## Q41.

Why do Docker permission problems happen frequently?

### Answer

UID/GID mismatches.

---

## Q42.

Why is:

```bash
docker run --privileged
```

dangerous?

### Answer

Container gains nearly full host privileges.

---

## Q43.

What Kubernetes setting improves security?

### Answer

```yaml
runAsNonRoot: true
```

---

## Q44.

Why should capabilities be dropped in containers?

### Answer

Reduce attack surface.

---

## Q45.

How do containers interact with Linux permissions?

### Answer

Containers still rely on:

```text
UID

GID

Ownership

Permissions

ACLs
```

---

# Section 10
# Scenario-Based Questions

---

## Q46.

Users can create files in a shared directory but group ownership is inconsistent.

What is missing?

### Answer

```text
SGID
```

---

## Q47.

Users can delete each other's files.

What should be configured?

### Answer

```text
Sticky Bit
```

---

## Q48.

An application cannot bind to port 80.

You don't want to run it as root.

What would you do?

### Answer

Grant:

```text
CAP_NET_BIND_SERVICE
```

---

## Q49.

A file shows:

```text
-rw-r-----
```

but another user can read it.

Why?

### Answer

Possible ACL entry.

Check:

```bash
getfacl
```

---

## Q50.

A user has correct permissions but still gets denied.

What are the most likely enterprise causes?

### Answer

```text
SELinux

AppArmor

ACL

Container Isolation

Filesystem Mount Options
```

---

# Rapid Fire Round

---

What command changes ownership?

```bash
chown
```

---

What command changes group ownership?

```bash
chgrp
```

---

View ACLs?

```bash
getfacl
```

---

Set ACL?

```bash
setfacl
```

---

View SELinux context?

```bash
ls -Z
```

---

View AppArmor status?

```bash
aa-status
```

---

Find SUID binaries?

```bash
find / -perm -4000
```

---

Find SGID binaries?

```bash
find / -perm -2000
```

---

View capabilities?

```bash
getcap
```

---

Restore SELinux labels?

```bash
restorecon
```

---

# Expert Challenge Questions

---

## Q51.

Design a secure shared directory for 100 developers.

Requirements:

```text
Shared Access

No Accidental Deletion

Consistent Group Ownership
```

Expected Answer:

```text
Groups

SGID

Sticky Bit

ACLs
```

---

## Q52.

How would you audit a Linux server for permission-related security risks?

Expected Areas:

```text
World Writable Files

SUID

SGID

ACLs

Capabilities

SELinux

AppArmor

Service Accounts
```

---

## Q53.

How would you secure a web server storing sensitive customer data?

Expected Concepts:

```text
Least Privilege

Dedicated Service Account

Restricted Permissions

ACLs

SELinux

Logging

Auditing
```

---

## Q54.

Explain the complete Linux access evaluation path.

Expected Flow:

```text
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
Access Decision
```

---

## Q55.

What are the most common causes of Linux permission outages in production?

### Answer

```text
Wrong Ownership

Directory Permissions

ACL Misconfiguration

SELinux Labels

Container UID Mismatch

Missing SGID

Missing Sticky Bit
```

---

# Final Interview Advice

Interviewers care less about:

```text
Memorizing chmod numbers
```

and more about:

```text
Troubleshooting

Security Thinking

Real-World Experience

Root Cause Analysis
```

If you can explain:

```text
Why access was denied
```

you are already ahead of most candidates.
