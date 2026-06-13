# SUID (Set User ID)

> SUID (Set User ID) is a special Linux permission that allows a program to run with the privileges of the file owner instead of the user executing it.

SUID is one of Linux's most powerful permission mechanisms.

It allows:

```text
Temporary Privilege Elevation
```

Without SUID, many essential Linux commands would not work.

Examples:

```text
passwd
su
sudo (internally related privilege handling)
mount (older systems)
ping (older systems)
```

---

# Learning Objectives

After reading this guide you will understand:

✅ What SUID is

✅ Why SUID exists

✅ Real UID vs Effective UID

✅ Linux execution model

✅ How SUID works internally

✅ How passwd works

✅ How privilege escalation occurs

✅ Security risks

✅ SUID exploitation techniques

✅ Auditing SUID binaries

✅ Hardening SUID systems

✅ Modern alternatives

---

# Why SUID Exists

Imagine a normal user:

```text
alice
```

Needs to change password.

Passwords are stored in:

```text
/etc/shadow
```

Permissions:

```bash
-r-------- root root
```

Only root can access.

---

# Problem

Alice needs:

```text
Modify Password
```

But:

```text
Cannot write /etc/shadow
```

---

# Solution

Linux allows:

```text
Run passwd with root privileges
```

without making Alice root.

This is exactly what SUID does.

---

# The Linux Execution Model

Normally:

```text
Program runs as
the user who launched it
```

Example:

```bash
whoami
```

Output:

```text
alice
```

Program privileges:

```text
alice privileges
```

---

# Normal Execution Flow

```text
User
 │
 ▼
Execute Program
 │
 ▼
Program Runs
 │
 ▼
User Privileges
```

---

# User IDs in Linux

Every process contains:

```text
Real UID (RUID)

Effective UID (EUID)

Saved UID

Filesystem UID
```

Most important:

```text
RUID
EUID
```

---

# Real UID

Represents:

```text
Who started the process
```

Example:

```text
alice
```

Real UID:

```text
1001
```

---

# Effective UID

Represents:

```text
Which permissions
the process currently uses
```

---

# Normal Process

```text
Real UID      = alice
Effective UID = alice
```

---

# SUID Process

```text
Real UID      = alice

Effective UID = root
```

Very different.

---

# Visual Representation

Without SUID:

```text
User: alice

Program
   │
   ▼

RUID = alice
EUID = alice
```

---

With SUID:

```text
User: alice

Program
   │
   ▼

RUID = alice
EUID = root
```

---

# How Linux Detects SUID

Linux checks:

```text
SUID Bit
```

during execution.

If set:

```text
EUID = File Owner
```

---

# Permission Layout

Normal:

```bash
-rwxr-xr-x
```

SUID:

```bash
-rwsr-xr-x
```

Notice:

```text
s replaces x
```

---

# Visual Breakdown

```text
-rwsr-xr-x

 │││
 ││└─ Execute
 │└── SUID
 └── Owner
```

---

# Setting SUID

Symbolic:

```bash
chmod u+s program
```

---

Numeric:

```bash
chmod 4755 program
```

---

# Why 4?

Special permission bits:

```text
4000 = SUID

2000 = SGID

1000 = Sticky Bit
```

---

# Example

```bash
chmod 4755 myprogram
```

Result:

```text
-rwsr-xr-x
```

---

# Removing SUID

```bash
chmod u-s program
```

or

```bash
chmod 755 program
```

---

# How passwd Works

One of Linux's most famous SUID examples.

---

# Step 1

User:

```text
alice
```

Runs:

```bash
passwd
```

---

# Step 2

Kernel loads:

```text
/usr/bin/passwd
```

---

# Step 3

File Owner:

```text
root
```

SUID enabled:

```text
YES
```

---

# Step 4

Kernel changes:

```text
EUID → root
```

---

# Step 5

passwd modifies:

```text
/etc/shadow
```

---

# Step 6

Program exits.

Privileges disappear.

---

# passwd Flowchart

```text
alice
   │
   ▼
passwd
   │
   ▼
SUID Detected
   │
   ▼
EUID = root
   │
   ▼
Modify /etc/shadow
   │
   ▼
Exit
```

---

# Inspecting SUID Files

Use:

```bash
ls -l
```

Example:

```bash
-rwsr-xr-x root root passwd
```

---

# Find All SUID Files

One of the most important admin commands.

```bash
find / -perm -4000 -type f 2>/dev/null
```

---

# Example Output

```text
/usr/bin/passwd

/usr/bin/su

/usr/bin/chsh

/usr/bin/gpasswd
```

---

# Real System SUID Programs

Common examples:

```text
passwd
su
chsh
chfn
gpasswd
mount (older systems)
umount (older systems)
```

---

# Why SUID Is Dangerous

SUID creates:

```text
Privilege Boundaries
```

If vulnerable:

```text
Privilege Escalation
```

becomes possible.

---

# Attack Scenario

Vulnerable SUID Program:

```text
Owned by root
```

Contains:

```c
system(user_input);
```

Attacker supplies:

```bash
/bin/bash
```

Program launches:

```text
Root Shell
```

---

# Visual Attack Flow

```text
Attacker
   │
   ▼
Vulnerable SUID Program
   │
   ▼
Code Execution
   │
   ▼
Root Shell
```

---

# Common SUID Vulnerabilities

---

## Buffer Overflow

Example:

```c
char buf[32];
gets(buf);
```

Can lead to:

```text
Root Code Execution
```

---

## Command Injection

Example:

```c
system(user_input);
```

Dangerous.

---

## Path Hijacking

Program runs:

```bash
cp file backup
```

without full path.

Attacker creates:

```text
Fake cp
```

Program executes attacker version.

---

## Environment Variable Abuse

Manipulate:

```text
PATH
LD_PRELOAD
LD_LIBRARY_PATH
```

Older programs often vulnerable.

---

# Auditing SUID Programs

---

# List SUID Programs

```bash
find / -perm -4000 -type f
```

---

# Verify Ownership

```bash
ls -l
```

Check:

```text
root ownership
```

---

# Check Package Origin

```bash
rpm -qf file

dpkg -S file
```

Unknown binaries:

```text
Investigate immediately
```

---

# Security Hardening

---

# Remove Unnecessary SUID

Bad:

```text
Unused SUID binaries
```

Remove:

```bash
chmod u-s binary
```

---

# Regular Audits

Schedule:

```bash
find / -perm -4000
```

Weekly.

---

# Restrict File Upload Locations

Never allow:

```text
User-controlled SUID files
```

---

# Use Modern Alternatives

Prefer:

```text
Linux Capabilities
```

instead of full root privileges.

---

# SUID vs sudo

Many people confuse them.

---

# SUID

```text
Permission Bit
```

Stored on file.

---

# sudo

```text
Privilege Delegation Framework
```

Controlled by:

```text
/etc/sudoers
```

---

# Comparison

| Feature | SUID | sudo |
|----------|---------|---------|
| File-based | Yes | No |
| User-specific | No | Yes |
| Logging | Minimal | Extensive |
| Granular Control | Limited | Excellent |
| Enterprise Preferred | No | Yes |

---

# SUID vs Capabilities

Traditional:

```text
Root = Everything
```

SUID often grants:

```text
Full root power
```

---

Capabilities:

```text
CAP_NET_ADMIN

CAP_NET_BIND_SERVICE

CAP_SYS_TIME
```

Granular permissions.

Safer.

---

# Modern Linux Trend

Old:

```text
SUID Everywhere
```

New:

```text
Capabilities
```

---

# Container Security

Most containers:

```text
Disable unnecessary SUID binaries
```

Reason:

```text
Container Escape Prevention
```

---

# Enterprise Best Practices

---

## Principle of Least Privilege

Avoid:

```text
Root SUID
```

unless required.

---

## Inventory SUID Programs

Maintain:

```text
Approved List
```

---

## Monitor Changes

Alert when:

```text
New SUID Binary Appears
```

---

## Use Capabilities

Where possible.

---

## Patch Frequently

Many privilege escalation vulnerabilities target SUID programs.

---

# Troubleshooting

---

## SUID Not Working

Check:

```bash
ls -l file
```

Verify:

```text
s present
```

---

## Permission Denied

Filesystem may be mounted:

```bash
nosuid
```

Check:

```bash
mount
```

---

## SUID Ignored

Common causes:

```text
nosuid mount option

NFS restrictions

Container security policy

SELinux
```

---

# Interview Questions

---

## What is SUID?

Allows a program to run with file owner's privileges.

---

## What does the SUID bit do?

Changes Effective UID during execution.

---

## Difference between Real UID and Effective UID?

Real UID:

```text
User who launched process
```

Effective UID:

```text
Permissions currently used
```

---

## Why does passwd need SUID?

Must modify:

```text
/etc/shadow
```

owned by root.

---

## How do you find SUID files?

```bash
find / -perm -4000 -type f
```

---

## Why is SUID dangerous?

Can lead to privilege escalation.

---

# Quick Cheat Sheet

```bash
chmod u+s file

chmod 4755 file

chmod u-s file

find / -perm -4000 -type f

ls -l

mount | grep nosuid
```

---

# Visual Summary

```text
Normal Program

alice
  │
  ▼
Program
  │
  ▼
EUID = alice
```

```text
SUID Program

alice
  │
  ▼
Program
  │
  ▼
EUID = root
```

```text
Real UID      = alice
Effective UID = root
```

---

# Key Takeaways

1. SUID enables temporary privilege elevation.

2. SUID changes Effective UID.

3. File owner privileges are used.

4. passwd relies on SUID.

5. SUID is powerful but dangerous.

6. Vulnerable SUID programs can lead to root compromise.

7. Regular auditing is essential.

8. Modern systems increasingly replace SUID with capabilities.

9. Enterprise environments carefully control SUID usage.

10. Understanding SUID is fundamental to Linux security.

---

# Next Step

Continue to:

```text
sgid.md
```

You will learn how Linux enables **group privilege inheritance**, shared team directories, collaborative development environments, and process group execution using the **Set Group ID (SGID)** permission bit.
