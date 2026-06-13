# chmod - Change File Permissions

> chmod (Change Mode) is the Linux command used to modify file and directory permissions.

It controls who can:

- Read files
- Modify files
- Execute programs
- Access directories

Understanding chmod is essential for:

- Linux Administration
- DevOps
- Cloud Engineering
- Cybersecurity
- System Programming
- Production Server Management

---

# Learning Objectives

After reading this document you will understand:

✅ What chmod does

✅ How Linux stores permissions

✅ Symbolic Mode

✅ Numeric (Octal) Mode

✅ Recursive Permission Changes

✅ File vs Directory Permissions

✅ Advanced chmod Operations

✅ Security Best Practices

✅ Production Use Cases

✅ Troubleshooting Permission Issues

✅ Common Mistakes

---

# What is chmod?

chmod modifies file permissions.

Syntax:

```bash
chmod [OPTIONS] MODE FILE
```

Example:

```bash
chmod 755 script.sh
```

Result:

```text
Owner   -> rwx
Group   -> r-x
Others  -> r-x
```

---

# Viewing Current Permissions

Before changing permissions:

```bash
ls -l
```

Example:

```bash
-rw-r--r-- file.txt
```

Breakdown:

```text
Owner   = rw-
Group   = r--
Others  = r--
```

---

# How Linux Represents Permissions

Linux internally uses:

```text
Read    = 4
Write   = 2
Execute = 1
```

---

# Permission Values

| Permission | Value |
|------------|--------|
| Read | 4 |
| Write | 2 |
| Execute | 1 |

---

# Permission Combinations

| Value | Permission |
|---------|------------|
| 0 | --- |
| 1 | --x |
| 2 | -w- |
| 3 | -wx |
| 4 | r-- |
| 5 | r-x |
| 6 | rw- |
| 7 | rwx |

---

# Two Ways to Use chmod

Linux supports:

```text
1. Symbolic Mode
2. Numeric (Octal) Mode
```

---

# Symbolic Mode

Uses letters:

```text
u = user (owner)

g = group

o = others

a = all
```

Operations:

```text
+ add permission

- remove permission

= set exact permission
```

---

# Add Permission

Example:

```bash
chmod u+x script.sh
```

Meaning:

```text
Add execute permission to owner
```

Before:

```text
rw-r--r--
```

After:

```text
rwxr--r--
```

---

# Remove Permission

Example:

```bash
chmod g-w file.txt
```

Meaning:

```text
Remove write permission from group
```

---

# Set Exact Permission

Example:

```bash
chmod o=r file.txt
```

Meaning:

```text
Others get only read permission
```

---

# Multiple Changes

Example:

```bash
chmod u+x,g+w,o-r file.txt
```

Meaning:

```text
Owner   -> add execute
Group   -> add write
Others  -> remove read
```

---

# Using "a"

Example:

```bash
chmod a+x script.sh
```

Meaning:

```text
Everyone gets execute permission
```

---

# Numeric Mode

Most common method in production.

Example:

```bash
chmod 755 script.sh
```

---

# How Numeric Mode Works

Remember:

```text
Read    = 4
Write   = 2
Execute = 1
```

---

# Example: 755

```text
7 = 4+2+1 = rwx

5 = 4+1   = r-x

5 = 4+1   = r-x
```

Result:

```text
rwxr-xr-x
```

---

# Visual Breakdown

```text
755

│││
││└── Others
│└──── Group
└───── Owner
```

---

# Example: 644

```bash
chmod 644 file.txt
```

Result:

```text
rw-r--r--
```

Meaning:

```text
Owner:
Read + Write

Group:
Read

Others:
Read
```

---

# Example: 600

```bash
chmod 600 secret.txt
```

Result:

```text
rw-------
```

Only owner can access.

---

# Example: 700

```bash
chmod 700 private.sh
```

Result:

```text
rwx------
```

Owner only.

---

# Common Permission Modes

| Mode | Meaning | Usage |
|--------|---------|--------|
| 777 | Everyone full access | Avoid |
| 755 | Executables | Scripts |
| 750 | Restricted executable | Services |
| 700 | Private executable | Personal scripts |
| 644 | Standard file | Configs |
| 640 | Restricted file | Internal configs |
| 600 | Sensitive data | SSH keys |

---

# File Permission Examples

---

## Text File

```bash
chmod 644 notes.txt
```

Recommended.

---

## Shell Script

```bash
chmod 755 backup.sh
```

Executable by everyone.

---

## SSH Private Key

```bash
chmod 600 id_rsa
```

Required.

---

# Directory Permissions

Directories behave differently.

---

# Directory Example

```text
project/
├── app.js
├── logs/
└── config/
```

Permission:

```bash
chmod 755 project
```

Meaning:

```text
Owner:
Read Write Execute

Group:
Read Execute

Others:
Read Execute
```

---

# Why Directories Need Execute Permission

Example:

```bash
cd project
```

Requires:

```text
Execute Permission
```

Without execute:

```text
Permission Denied
```

---

# Recursive chmod

Change permissions recursively.

Syntax:

```bash
chmod -R MODE DIRECTORY
```

Example:

```bash
chmod -R 755 project
```

Applies to:

```text
All files
All subdirectories
```

---

# Recursive Permission Visualization

```text
project/
├── file1
├── file2
└── src
    ├── app.js
    └── util.js
```

Command:

```bash
chmod -R 755 project
```

Everything changes.

---

# Production Warning

Avoid:

```bash
chmod -R 777 /
```

Catastrophic security risk.

---

# Advanced chmod Operations

---

# Copy Permissions

Copy permissions from another file.

```bash
chmod --reference=file1 file2
```

Example:

```bash
chmod --reference=config.old config.new
```

---

# Verbose Mode

```bash
chmod -v 755 file.txt
```

Output:

```text
mode changed
```

Useful for automation.

---

# Change Only When Necessary

```bash
chmod -c 755 file.txt
```

Only reports actual changes.

---

# Preserving Symlinks

Linux supports symbolic links.

Example:

```bash
ln -s file1 link1
```

Be careful when recursively changing permissions.

---

# Real World Examples

---

# Web Server

```text
/var/www/html
```

Directories:

```bash
755
```

Files:

```bash
644
```

---

# SSH

```text
~/.ssh
```

Directory:

```bash
700 ~/.ssh
```

Private key:

```bash
600 ~/.ssh/id_rsa
```

---

# Database Config

```text
db.conf
```

Permission:

```bash
640 db.conf
```

---

# Docker Volume

Example:

```bash
chmod 750 app-data
```

Only authorized services can access.

---

# Security Best Practices

---

## Principle of Least Privilege

Grant only required permissions.

Good:

```text
640
750
755
644
```

Bad:

```text
777
```

---

## Avoid World Writable Files

Bad:

```bash
chmod 777 file.txt
```

Anyone can modify.

---

## Restrict Sensitive Files

Use:

```bash
chmod 600
```

For:

```text
SSH Keys
Passwords
Secrets
Certificates
```

---

## Audit Permissions

Find world writable files:

```bash
find / -type f -perm -002
```

---

# Common Mistakes

---

## Mistake #1

```bash
chmod 777 everything
```

Wrong solution.

---

## Mistake #2

Removing execute from directories.

```bash
chmod 644 mydir
```

Result:

```text
Cannot enter directory
```

---

## Mistake #3

Making data files executable.

```bash
chmod 755 notes.txt
```

Unnecessary.

---

## Mistake #4

Recursive changes without verification.

```bash
chmod -R 777 project
```

Dangerous.

---

# Troubleshooting

---

## Permission Denied

Check:

```bash
ls -l
```

---

## Cannot Execute Script

Verify:

```bash
chmod +x script.sh
```

---

## Cannot Access Directory

Verify:

```bash
ls -ld directory
```

Ensure execute permission exists.

---

## SSH Key Rejected

Check:

```bash
chmod 600 ~/.ssh/id_rsa
```

SSH refuses weak permissions.

---

# Interview Questions

### What does chmod do?

Changes file or directory permissions.

---

### Difference between 755 and 644?

755:

```text
Executable
```

644:

```text
Non-executable file
```

---

### Why is 777 dangerous?

Everyone gets full access.

---

### Why does a directory need execute permission?

To enter/traverse the directory.

---

### What does chmod -R do?

Applies changes recursively.

---

# Quick Cheat Sheet

```bash
chmod 755 script.sh
chmod 644 file.txt
chmod 600 secret.txt
chmod 700 private.sh

chmod u+x script.sh
chmod g-w file.txt
chmod o-r file.txt

chmod -R 755 project/

chmod --reference=file1 file2
```

---

# Key Takeaways

```text
1. chmod changes permissions.

2. Two modes:
   Symbolic
   Numeric

3. Read = 4
   Write = 2
   Execute = 1

4. 755 and 644 are most common.

5. Directories require execute permission.

6. Recursive changes must be used carefully.

7. Never use 777 unless absolutely necessary.

8. Least Privilege is the best security practice.
```

---

# Next Step

Continue to:

```text
chown.md
```

Where you will learn how Linux ownership works and how ownership affects permission evaluation.
