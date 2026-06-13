# Linux Permissions & Security

> "Everything in Linux is a file, and every file has permissions."

Permissions are one of the most important concepts in Linux.

Every action performed in Linux is controlled by permissions:

- Reading files
- Editing files
- Executing programs
- Accessing devices
- Managing services
- Using network ports
- Accessing databases
- Running administrative commands

Understanding permissions is essential for:

- Linux Administration
- DevOps
- Cloud Engineering
- Cybersecurity
- System Programming
- Site Reliability Engineering (SRE)

---

# Learning Objectives

After completing this section you will understand:

✅ Linux Permission Model

✅ User Ownership

✅ Group Ownership

✅ Read / Write / Execute Permissions

✅ chmod

✅ chown

✅ chgrp

✅ umask

✅ SUID

✅ SGID

✅ Sticky Bit

✅ Access Control Lists (ACL)

✅ Linux Capabilities

✅ SELinux

✅ AppArmor

✅ Security Hardening

✅ Permission Troubleshooting

✅ Real Production Security Practices

---

# Why Linux Needs Permissions

Imagine a server containing:

```text
/home
├── alice
├── bob
├── database
└── backups
```

Without permissions:

- Alice could delete Bob's files
- Applications could modify system files
- Malware could destroy data
- Any user could become root

Linux solves this using:

1. Ownership
2. Permissions
3. Security Policies

---

# Linux Security Layers

```text
┌──────────────────────────────┐
│ User                         │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ Traditional Permissions      │
│ rwx                          │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ ACLs                         │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ Capabilities                 │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ SELinux / AppArmor           │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ Kernel Enforcement           │
└──────────────────────────────┘
```

Every file access request passes through these layers.

---

# Permission System Overview

Linux permissions are based on:

```text
WHO can access
+
WHAT action can be performed
```

---

# The Three Permission Classes

Every file has:

```text
Owner
Group
Others
```

Example:

```bash
-rwxr-xr--
```

Breakdown:

```text
Owner   Group   Others
rwx     r-x     r--

```

---

# Permission Meanings

## Read (r)

Value:

```text
4
```

Allows:

- View file contents
- Open file

Example:

```bash
cat notes.txt
```

---

## Write (w)

Value:

```text
2
```

Allows:

- Modify file
- Delete file contents
- Append data

Example:

```bash
echo "hello" >> file.txt
```

---

## Execute (x)

Value:

```text
1
```

Allows:

- Run program
- Execute script

Example:

```bash
./script.sh
```

---

# Numeric Permission System

Linux converts permissions into numbers.

```text
Read      = 4
Write     = 2
Execute   = 1
```

Examples:

```text
7 = 4+2+1 = rwx
6 = 4+2   = rw-
5 = 4+1   = r-x
4 = 4     = r--
```

---

# Common Permission Sets

| Numeric | Symbolic | Meaning |
|----------|-----------|----------|
| 777 | rwxrwxrwx | Everyone full access |
| 755 | rwxr-xr-x | Typical executable |
| 644 | rw-r--r-- | Typical file |
| 700 | rwx------ | Private |
| 600 | rw------- | Sensitive file |
| 400 | r-------- | Read only |

---

# Visual Permission Breakdown

```text
-rwxr-xr--

│││ │││ │││
│││ │││ │└┴ Others
│││ └┴┴──── Group
└┴┴──────── Owner
```

---

# Ownership

Every file belongs to:

```text
User Owner
Group Owner
```

Example:

```bash
-rw-r--r-- alice developers file.txt
```

Meaning:

Owner:
    alice

Group:
    developers
```

---

# Permission Decision Flow

When a user accesses a file:

```text
User requests access
        │
        ▼
Is user owner?
        │
   Yes / No
        │
        ▼
Check owner permissions
        │
        ▼
If not owner
        │
        ▼
Check group permissions
        │
        ▼
If not group
        │
        ▼
Check others permissions
        │
        ▼
Allow / Deny
```

---

# Directories Work Differently

For directories:

| Permission | Meaning |
|------------|----------|
| r | List contents |
| w | Create/Delete files |
| x | Enter directory |

Example:

```bash
cd /data
```

Requires:

```text
x permission
```

---

# Real Directory Example

```text
project/
├── code.py
├── logs/
└── config/
```

Permission:

```bash
drwxr-x---
```

Means:

Owner:
    Full access

Group:
    Read + Enter

Others:
    No access
```

---

# Important Commands

This section covers:

```text
chmod
chown
chgrp
umask
```

Detailed explanations exist in separate files.

---

# Advanced Permission Features

Linux permissions extend beyond rwx.

---

## SUID

Allows program to run with owner's privileges.

Example:

```bash
passwd
```

Normal users can update passwords because:

```text
passwd has SUID
```

---

## SGID

Used for:

- Shared directories
- Group inheritance

Useful in:

```text
Development Teams
Shared Projects
```

---

## Sticky Bit

Prevents users from deleting files they don't own.

Used on:

```text
/tmp
```

Example:

```bash
drwxrwxrwt
```

---

# Access Control Lists (ACL)

Traditional permissions:

```text
Owner
Group
Others
```

Sometimes not enough.

Example:

Need:

```text
alice = read
bob = read/write
charlie = no access
```

ACL solves this.

---

# Linux Capabilities

Traditionally:

```text
Root = all powers
```

Problem:

Too dangerous.

Capabilities split root privileges into smaller pieces.

Example:

```text
CAP_NET_ADMIN
CAP_SYS_ADMIN
CAP_NET_BIND_SERVICE
```

Applications receive only required privileges.

---

# SELinux

SELinux adds:

```text
Mandatory Access Control
```

Even if file permissions allow access:

```text
SELinux can deny it.
```

Used heavily in:

- RHEL
- CentOS
- Rocky Linux
- Enterprise Systems

---

# AppArmor

Alternative to SELinux.

Profile-based security system.

Popular in:

- Ubuntu
- Debian

Example:

```text
Firefox Profile
Nginx Profile
Docker Profile
```

---

# Real World Permission Architecture

Web Server Example:

```text
Nginx
 │
 ▼
/var/www/html
 │
 ▼
Permissions
 │
 ▼
SELinux
 │
 ▼
Kernel
```

All layers must allow access.

---

# Permission Troubleshooting Workflow

When access fails:

```text
1. Check owner
2. Check group
3. Check permissions
4. Check ACL
5. Check capabilities
6. Check SELinux
7. Check AppArmor
8. Check filesystem mount options
```

---

# Common Beginner Mistakes

## Using 777 Everywhere

Bad:

```bash
chmod 777 file.txt
```

Reason:

Everyone gets full control.

---

## Running Everything as Root

Bad:

```bash
sudo su
```

Use least privilege.

---

## Incorrect Ownership

Example:

```bash
root owns application files
```

Application cannot write logs.

---

## Ignoring SELinux

Many administrators disable SELinux.

Wrong solution.

Learn policy management instead.

---

# Security Best Practices

## Principle of Least Privilege

Give only required permissions.

---

## Use Groups

Instead of:

```text
Managing users individually
```

Use:

```text
Groups
```

---

## Avoid 777

Prefer:

```text
644
755
750
640
```

---

## Audit Permissions

Regularly review:

```bash
find / -perm -4000
find / -perm -2000
```

---

## Use ACLs Carefully

Only when traditional permissions are insufficient.

---

## Keep SELinux Enabled

Enterprise recommendation:

```text
Enforcing Mode
```

---

# Production Examples

## Web Servers

```text
Nginx
Apache
```

Use:

```text
755 directories
644 files
```

---

## SSH Keys

```bash
~/.ssh/id_rsa
```

Permissions:

```text
600
```

---

## Databases

```text
MySQL
PostgreSQL
```

Use restricted ownership.

---

## Docker

Containers use:

- namespaces
- capabilities
- cgroups
- permissions

Together for isolation.

---

# File Structure

```text
06-permissions/
│
├── README.md
├── permission-model.md
├── chmod.md
├── chown.md
├── chgrp.md
├── umask.md
├── suid.md
├── sgid.md
├── sticky-bit.md
├── acl.md
├── capabilities.md
├── selinux-basics.md
├── apparmor-basics.md
├── interview-questions.md
└── references.md
```

---

# Learning Path

Recommended order:

```text
1. Permission Model
2. chmod
3. chown
4. chgrp
5. umask
6. SUID
7. SGID
8. Sticky Bit
9. ACL
10. Capabilities
11. SELinux
12. AppArmor
13. Security Hardening
14. Interview Questions
```

---

# What Comes Next?

After mastering permissions you should continue with:

```text
07-process-management/
08-networking/
09-storage-management/
10-systemd/
11-security/
```

Permissions are the foundation of Linux security.

Master them before moving into advanced administration.
