# Linux Permissions Interview Questions (Part 3)

> Expert-Level Linux Permission, Security, Enterprise Administration, RHCSA/RHCE, Security Engineering, and Production Troubleshooting Questions.

---

# Section 1
# Permission Internals

---

## Q1.

Where are Linux permissions stored?

### Answer

Permissions are stored inside:

```text
Inode Metadata
```

Every file has an inode containing:

```text
Owner UID

Group GID

Permission Bits

Timestamps

File Size

Extended Attributes
```

Visualization:

```text
File
 │
 ▼
Inode
 │
 ├── UID
 ├── GID
 ├── rwx
 ├── ACL
 └── Metadata
```

---

## Q2.

What system call is used to check access permissions?

### Answer

Common system calls:

```c
access()

faccessat()
```

Kernel ultimately evaluates credentials.

---

## Q3.

How does Linux determine whether access is allowed?

### Answer

Simplified flow:

```text
Process Credentials
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
Capabilities
       │
       ▼
LSM (SELinux/AppArmor)
       │
       ▼
Decision
```

---

## Q4.

What credentials are associated with a process?

### Answer

```text
UID

EUID

GID

EGID

Supplementary Groups

Capabilities
```

---

## Q5.

Difference between UID and EUID?

### Answer

UID:

```text
Real User
```

EUID:

```text
Effective User
```

Used during permission checks.

---

Example:

```text
alice executes passwd
```

```text
UID  = alice

EUID = root
```

due to SUID.

---

# Section 2
# Kernel Permission Model

---

## Q6.

What happens internally when a process opens a file?

### Answer

Kernel workflow:

```text
open()
   │
   ▼
Path Resolution
   │
   ▼
Inode Lookup
   │
   ▼
Permission Evaluation
   │
   ▼
LSM Check
   │
   ▼
File Descriptor
```

---

## Q7.

What is VFS?

### Answer

Virtual File System.

Provides common interface for:

```text
ext4

xfs

btrfs

nfs

tmpfs
```

---

## Q8.

Where are ACLs stored?

### Answer

Stored as:

```text
Extended Attributes
```

(xattrs)

---

## Q9.

Why are ACLs more flexible than traditional permissions?

### Answer

Traditional model:

```text
Owner

Group

Others
```

ACL model:

```text
Many Users

Many Groups
```

---

## Q10.

What is an inode?

### Answer

Data structure describing a file.

Contains metadata.

Not filename.

---

# Section 3
# SUID Internals

---

## Q11.

What happens internally when a SUID binary executes?

### Answer

Kernel changes:

```text
Effective UID
```

to:

```text
Owner UID
```

of executable.

---

Flow:

```text
User
 │
 ▼
SUID Program
 │
 ▼
Kernel
 │
 ▼
Change EUID
 │
 ▼
Execute
```

---

## Q12.

Why is SUID dangerous?

### Answer

Vulnerabilities become:

```text
Privilege Escalation
```

opportunities.

---

## Q13.

What common vulnerabilities affect SUID binaries?

### Answer

```text
Buffer Overflow

Command Injection

Path Hijacking

Race Conditions
```

---

## Q14.

Why is SUID shell considered dangerous?

### Answer

May grant:

```text
Root Shell
```

directly.

---

## Q15.

How do modern systems reduce SUID usage?

### Answer

Using:

```text
Capabilities
```

instead.

---

# Section 4
# Linux Capabilities Deep Dive

---

## Q16.

Why were capabilities introduced?

### Answer

Root privilege was too broad.

Capabilities divide privileges into smaller units.

---

## Q17.

What capability allows changing file ownership?

### Answer

```text
CAP_CHOWN
```

---

## Q18.

What capability allows modifying system time?

### Answer

```text
CAP_SYS_TIME
```

---

## Q19.

What capability allows network administration?

### Answer

```text
CAP_NET_ADMIN
```

---

## Q20.

Why is CAP_SYS_ADMIN dangerous?

### Answer

Contains enormous privilege scope.

Often described as:

```text
The New Root
```

---

## Q21.

Name capability sets.

### Answer

```text
Permitted

Effective

Inheritable

Bounding

Ambient
```

---

## Q22.

What is Bounding Set?

### Answer

Upper limit of capabilities a process may obtain.

---

## Q23.

What is Ambient Capability?

### Answer

Capability preserved across exec().

---

# Section 5
# SELinux Deep Dive

---

## Q24.

What security model does SELinux implement?

### Answer

```text
Mandatory Access Control (MAC)
```

---

## Q25.

Difference between DAC and MAC?

### Answer

DAC:

```text
Owner Controlled
```

MAC:

```text
Policy Controlled
```

---

## Q26.

What is Type Enforcement?

### Answer

Core SELinux mechanism.

Controls:

```text
Domain
     ↓
Type
```

access.

---

## Q27.

What is a SELinux context?

### Answer

Format:

```text
user:role:type:level
```

Example:

```text
system_u:object_r:httpd_sys_content_t:s0
```

---

## Q28.

Most important SELinux field?

### Answer

```text
Type
```

---

## Q29.

What is AVC?

### Answer

```text
Access Vector Cache
```

Caches SELinux decisions.

---

## Q30.

How do you investigate AVC denials?

### Answer

```bash
ausearch -m avc
```

---

## Q31.

What is restorecon?

### Answer

Restores default SELinux labels.

---

## Q32.

What is semanage?

### Answer

SELinux policy management utility.

---

# Section 6
# AppArmor Deep Dive

---

## Q33.

Why is AppArmor considered simpler than SELinux?

### Answer

Uses:

```text
Path-Based Profiles
```

instead of labels.

---

## Q34.

What are AppArmor modes?

### Answer

```text
Enforce

Complain

Disable
```

---

## Q35.

Where are AppArmor profiles stored?

### Answer

```text
/etc/apparmor.d/
```

---

# Section 7
# Enterprise Security

---

## Q36.

What is Least Privilege?

### Answer

Only required permissions granted.

---

## Q37.

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

## Q38.

What is Separation of Duties?

### Answer

Different responsibilities.

Example:

```text
Developers

Administrators

Security Team
```

---

## Q39.

Why should service accounts be used?

### Answer

Limit impact of compromise.

---

## Q40.

What permissions should SSH private keys use?

### Answer

```text
600
```

---

# Section 8
# Containers

---

## Q41.

How are containers related to Linux permissions?

### Answer

Containers still use:

```text
UID

GID

Permissions

ACLs
```

---

## Q42.

Why do volume permission issues occur?

### Answer

UID mismatch.

---

## Q43.

What Linux technologies power containers?

### Answer

```text
Namespaces

cgroups

Capabilities
```

---

## Q44.

Why avoid privileged containers?

### Answer

Near-host-root privileges.

---

## Q45.

What Kubernetes security feature maps group ownership?

### Answer

```yaml
fsGroup
```

---

# Section 9
# Architecture Questions

---

## Q46.

Design secure shared storage for 100 developers.

Expected Answer:

```text
Groups

SGID

Sticky Bit

ACL

SELinux
```

---

## Q47.

Design secure web hosting.

Expected Components:

```text
Dedicated Service Account

Restricted Permissions

SELinux

Least Privilege
```

---

## Q48.

Design multi-tenant Linux server.

Expected Concepts:

```text
Users

Groups

ACL

Namespaces

SELinux
```

---

# Section 10
# Production Incidents

---

## Q49.

A website suddenly returns 403.

Permissions look correct.

What next?

### Answer

Investigate:

```text
SELinux

ACL

Directory Permissions
```

---

## Q50.

A containerized application cannot write logs.

Most likely cause?

### Answer

```text
UID/GID mismatch
```

---

## Q51.

Users accidentally delete each other's files.

Solution?

### Answer

```text
Sticky Bit
```

---

## Q52.

Shared project files get inconsistent groups.

Solution?

### Answer

```text
SGID
```

---

## Q53.

Application needs port 80 without root.

Solution?

### Answer

```text
CAP_NET_BIND_SERVICE
```

---

## Q54.

Server compromised through vulnerable SUID binary.

What failed?

### Answer

```text
Privilege Escalation Protection
```

---

## Q55.

Why do enterprises keep SELinux enabled?

### Answer

Even if permissions are wrong:

```text
SELinux may still prevent compromise.
```

---

# RHCSA / RHCE Favorites

---

## Q56.

Difference:

```bash
chmod 755
```

vs

```bash
chmod u=rwx,g=rx,o=rx
```

### Answer

Same result.

Different syntax.

---

## Q57.

How do you view SELinux status?

```bash
sestatus
```

---

## Q58.

How do you view contexts?

```bash
ls -Z
```

---

## Q59.

How do you find SUID files?

```bash
find / -perm -4000
```

---

## Q60.

How do you find world-writable files?

```bash
find / -perm -002
```

---

# Expert Rapid Fire

---

Where are permissions stored?

```text
Inode
```

---

Where are ACLs stored?

```text
Extended Attributes
```

---

Most dangerous capability?

```text
CAP_SYS_ADMIN
```

---

Most common SELinux issue?

```text
Wrong Labels
```

---

Most common Docker permission issue?

```text
UID Mismatch
```

---

Best security principle?

```text
Least Privilege
```

---

# Interviewer's Hidden Goal

Expert interviewers are not testing:

```text
chmod memorization
```

They are testing:

```text
System Thinking

Security Mindset

Troubleshooting Ability

Architecture Design

Root Cause Analysis
```

A candidate who can explain:

```text
WHY access was denied
```

is far more valuable than someone who can only recite permission numbers.
