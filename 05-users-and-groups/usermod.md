# ✏️ `usermod` in Linux — Complete Beginner to Professional Guide

> Learn how Linux modifies existing user accounts, changes usernames, updates groups, moves home directories, changes shells, locks accounts, and manages identities in enterprise environments.

---

# 🎯 What is `usermod`?

Imagine a school student already exists in the school's database.

Now suppose:

```text
Rahul changes his name
Rahul moves to another class
Rahul changes his locker
Rahul graduates
Rahul gets special permissions
```

The school does **not create a new student record**.

Instead it modifies the existing one.

Linux does the same thing using:

```bash
usermod
```

---

# 🧠 Simple Definition

`usermod` modifies an existing user account.

Think:

```text
useradd → Create User

usermod → Modify User

userdel → Delete User
```

Visual:

```text
User Lifecycle

Create
  │
  ▼
useradd
  │
  ▼
Existing User
  │
  ▼
usermod
  │
  ▼
Updated User
  │
  ▼
userdel
  │
  ▼
Deleted User
```

---

# 🌍 Why Do We Need `usermod`?

In real systems users constantly change.

Examples:

```text
Employee promoted
Department changed
Username corrected
Home directory moved
Shell changed
Temporary access granted
```

Creating a new account every time would be a disaster.

Instead:

```text
Modify Existing User
```

---

# 🚀 Basic Syntax

```bash
usermod [options] username
```

Example:

```bash
sudo usermod -s /bin/zsh rahul
```

Changes Rahul's shell.

---

# 🔍 What Happens Internally?

When `usermod` runs, Linux may update:

```text
/etc/passwd
/etc/shadow
/etc/group
/home/user
```

Depending on the options used.

Visual:

```text
usermod
   │
   ▼
Check User
   │
   ▼
Modify Settings
   │
   ▼
Update Databases
   │
   ▼
Save Changes
```

---

# 🏗️ Understanding User Identity

Before learning commands, understand that a Linux user has:

```text
Username
UID
Primary Group
Secondary Groups
Home Directory
Shell
Password Settings
Account Status
```

`usermod` can change almost all of them.

---

# 👤 Change Username

Suppose:

```text
rahul
```

needs to become:

```text
rahul_sharma
```

Use:

```bash
sudo usermod -l rahul_sharma rahul
```

Option:

```text
-l = login name
```

Visual:

```text
Before

rahul

After

rahul_sharma
```

---

# Verify

```bash
id rahul_sharma
```

---

# ⚠️ Important

Changing username does NOT automatically rename:

```text
/home/rahul
```

You must handle that separately.

---

# 🏠 Change Home Directory

Current:

```text
/home/rahul
```

New:

```text
/projects/rahul
```

Command:

```bash
sudo usermod -d /projects/rahul rahul
```

Option:

```text
-d = home directory
```

---

# What Happens?

Linux changes:

```text
/etc/passwd
```

from:

```text
/home/rahul
```

to:

```text
/projects/rahul
```

Only the path changes.

Files are NOT moved automatically.

---

# Move Existing Files Too

Use:

```bash
sudo usermod -d /projects/rahul -m rahul
```

Option:

```text
-m = move contents
```

Visual:

```text
Before

/home/rahul
    │
    ├── Documents
    ├── Downloads
    └── Projects

After

/projects/rahul
    │
    ├── Documents
    ├── Downloads
    └── Projects
```

---

# 🖥️ Change Login Shell

Current:

```text
/bin/bash
```

New:

```text
/bin/zsh
```

Command:

```bash
sudo usermod -s /bin/zsh rahul
```

Option:

```text
-s = shell
```

Visual:

```text
User
 │
 ▼
bash
```

becomes

```text
User
 │
 ▼
zsh
```

---

# Verify Shell

```bash
getent passwd rahul
```

Example:

```text
rahul:x:1001:1001::/home/rahul:/bin/zsh
```

---

# 🆔 Change UID

Current:

```text
UID 1001
```

New:

```text
UID 2000
```

Command:

```bash
sudo usermod -u 2000 rahul
```

Option:

```text
-u = UID
```

---

# Why Change UID?

Common in:

```text
NFS
Clusters
Shared Storage
Enterprise Migrations
```

---

# ⚠️ UID Warning

Files owned by:

```text
UID 1001
```

may still remain owned by old UID.

Always verify file ownership after changing UID.

---

# 👥 Change Primary Group

Current:

```text
developers
```

New:

```text
engineering
```

Command:

```bash
sudo usermod -g engineering rahul
```

Option:

```text
-g = primary group
```

Visual:

```text
Before

Rahul
  │
  ▼
developers
```

After:

```text
Rahul
  │
  ▼
engineering
```

---

# 👥 Add Secondary Groups

Current:

```text
developers
```

Need:

```text
developers
docker
sudo
git
```

Command:

```bash
sudo usermod -aG docker,sudo,git rahul
```

Options:

```text
-a = append
-G = secondary groups
```

---

# Visual

```text
            Rahul
               │
     ┌─────────┼─────────┐
     │         │         │
 developers  docker    sudo
```

---

# 🚨 Most Common Linux Mistake

Wrong:

```bash
sudo usermod -G docker rahul
```

This replaces existing secondary groups.

---

Example:

Before:

```text
developers
sudo
git
```

Run:

```bash
sudo usermod -G docker rahul
```

After:

```text
docker
```

Everything else removed.

---

# Correct Method

```bash
sudo usermod -aG docker rahul
```

Always remember:

```text
-aG
```

when adding groups.

---

# Remove User from Groups

Linux does not have a direct:

```bash
usermod --remove-group
```

Instead use:

```bash
sudo gpasswd -d rahul docker
```

---

# 🔒 Lock Account

Temporarily disable login.

Command:

```bash
sudo usermod -L rahul
```

Option:

```text
-L = lock
```

Visual:

```text
User
 │
 ▼
LOCKED
 │
 ▼
Login Denied
```

---

# What Happens Internally?

Linux modifies:

```text
/etc/shadow
```

Password hash becomes:

```text
!$6$...
```

Meaning:

```text
Account Locked
```

---

# 🔓 Unlock Account

Command:

```bash
sudo usermod -U rahul
```

Option:

```text
-U = unlock
```

Visual:

```text
LOCKED
  │
  ▼
UNLOCKED
  │
  ▼
Login Allowed
```

---

# 📅 Set Account Expiration

Temporary employee:

```bash
sudo usermod -e 2027-12-31 rahul
```

Option:

```text
-e = expiration date
```

Visual:

```text
Account Active
       │
       ▼
31 Dec 2027
       │
       ▼
Disabled
```

---

# Remove Expiration

Make permanent:

```bash
sudo usermod -e "" rahul
```

---

# 📄 Change User Comment

Current:

```text
Rahul
```

New:

```text
Rahul Sharma - DevOps Engineer
```

Command:

```bash
sudo usermod -c "Rahul Sharma - DevOps Engineer" rahul
```

Option:

```text
-c = comment
```

Stored in:

```text
/etc/passwd
```

GECOS field.

---

# 🏗️ Enterprise Example

New employee:

```text
Rahul
```

After promotion:

```text
Department → DevOps
Shell → zsh
Groups → docker,sudo,kubernetes
```

Command:

```bash
sudo usermod \
-s /bin/zsh \
-g devops \
-aG docker,sudo,kubernetes \
rahul
```

One command updates everything.

---

# 🔍 Verify User Information

---

## Show Identity

```bash
id rahul
```

Example:

```text
uid=1001(rahul)
gid=1001(devops)
groups=1001(devops),27(sudo),999(docker)
```

---

## Show User Entry

```bash
getent passwd rahul
```

---

## Check Groups

```bash
groups rahul
```

---

## Check Shadow Status

```bash
sudo chage -l rahul
```

---

# 🔄 What Files Can Be Modified?

| Option | File Affected |
| ------ | ------------- |
| -l     | /etc/passwd   |
| -u     | /etc/passwd   |
| -g     | /etc/passwd   |
| -G     | /etc/group    |
| -s     | /etc/passwd   |
| -d     | /etc/passwd   |
| -L     | /etc/shadow   |
| -U     | /etc/shadow   |
| -e     | /etc/shadow   |

---

# 🧠 Real-World Scenarios

---

## Employee Name Changed

```bash
usermod -l newname oldname
```

---

## Department Transfer

```bash
usermod -g engineering rahul
```

---

## Grant Docker Access

```bash
usermod -aG docker rahul
```

---

## Disable Contractor Account

```bash
usermod -L contractor
```

---

## Move User Data

```bash
usermod -d /data/rahul -m rahul
```

---

# 🚨 Common Mistakes

---

## Forgetting -a

Wrong:

```bash
usermod -G docker rahul
```

Removes existing groups.

---

Correct:

```bash
usermod -aG docker rahul
```

---

## Changing UID Without Ownership Fix

Wrong:

```bash
usermod -u 2000 rahul
```

Without checking files.

Result:

```text
Orphaned File Ownership
```

---

## Changing Home Without -m

Wrong:

```bash
usermod -d /newhome/rahul rahul
```

Files stay in old location.

---

Better:

```bash
usermod -d /newhome/rahul -m rahul
```

---

# ⚡ Most Useful Options

| Option | Meaning              |
| ------ | -------------------- |
| -l     | Change username      |
| -u     | Change UID           |
| -g     | Change primary group |
| -G     | Secondary groups     |
| -a     | Append groups        |
| -d     | Home directory       |
| -m     | Move home contents   |
| -s     | Login shell          |
| -c     | Comment field        |
| -e     | Expiration date      |
| -L     | Lock account         |
| -U     | Unlock account       |

---

# 🧠 Interview Questions

### What does `usermod` do?

Modifies existing user accounts.

---

### Difference between `useradd` and `usermod`?

```text
useradd → Create User

usermod → Modify User
```

---

### What does `-aG` mean?

```text
-a → Append

-G → Secondary Groups
```

Used together to safely add groups.

---

### How do you change a user's shell?

```bash
usermod -s /bin/zsh username
```

---

### How do you lock an account?

```bash
usermod -L username
```

---

### How do you unlock an account?

```bash
usermod -U username
```

---

### How do you move a home directory?

```bash
usermod -d /new/path -m username
```

---

### What is the biggest mistake with `usermod`?

Using:

```bash
usermod -G group username
```

instead of:

```bash
usermod -aG group username
```

---

# 🎯 Summary

`usermod` is Linux's account modification tool.

It allows administrators to:

```text
Change Usernames
Change UIDs
Manage Groups
Move Home Directories
Change Shells
Lock Accounts
Unlock Accounts
Set Expiration Policies
```

Understanding `usermod` is essential for:

```text
Linux Administration
DevOps
Cloud Engineering
Cybersecurity
Enterprise Identity Management
```

---

# 🗺️ Big Picture

```text
Create User
    │
    ▼
useradd
    │
    ▼
Modify User
    │
    ▼
usermod
    │
    ▼
Manage Lifecycle
    │
    ▼
userdel
```

---

# 📚 Next File

Continue with:

```text
userdel.md
```

You will learn:

✅ How Linux deletes users

✅ What happens to files after deletion

✅ Home directory cleanup

✅ UID ownership issues

✅ Safe user removal practices

✅ Enterprise deprovisioning workflows
