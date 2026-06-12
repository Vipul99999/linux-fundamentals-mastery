# 👤 User Management in Linux

> Learn how Linux creates, identifies, manages, and controls users.

---

# 🎯 What is User Management?

Imagine a large apartment building.

Every resident has:

* A name
* An apartment number
* A key
* Access permissions

Linux works similarly.

Every person who uses Linux gets:

```text
Username
Password
User ID (UID)
Home Directory
Shell
Permissions
```

Linux uses these details to identify and manage users.

---

# 🌍 Why User Management Exists

Without user management:

```text
Anyone could access files
Anyone could delete data
Anyone could change settings
Anyone could break the system
```

Linux prevents this by creating separate user accounts.

Example:

```text
Linux System

├── Rahul
├── Priya
├── Aman
└── Admin
```

Each user gets their own space.

---

# 🧠 What is a User?

A user is an identity recognized by Linux.

Example:

```text
rahul
```

Linux stores information about this user.

```text
Username : rahul
UID      : 1001
Group    : developers
Home     : /home/rahul
Shell    : /bin/bash
```

Think of it as:

```text
Student ID Card

Name        : Rahul
Roll Number : 1001
Class       : Developers
Locker      : /home/rahul
```

---

# 🆔 User ID (UID)

Linux internally identifies users using numbers.

Not names.

Example:

```text
rahul = UID 1001
priya = UID 1002
aman  = UID 1003
```

Linux actually sees:

```text
UID 1001 owns file.txt
```

instead of:

```text
rahul owns file.txt
```

---

# 🎨 Why Linux Uses UID Instead of Names

Names can change.

Example:

```text
rahul → rahul_sharma
```

UID remains:

```text
1001
```

Because numbers never change accidentally, Linux uses them internally.

---

# 🔢 Important UID Ranges

Different Linux distributions may vary slightly, but generally:

| UID Range | Purpose      |
| --------- | ------------ |
| 0         | Root User    |
| 1-999     | System Users |
| 1000+     | Normal Users |

Visual:

```text
UID Space

0
│
├── Root
│
1-999
│
├── System Accounts
│
1000+
│
└── Human Users
```

---

# 👑 Root User

The most powerful user.

```text
Username : root
UID      : 0
```

Can:

✅ Install software

✅ Create users

✅ Delete users

✅ Manage hardware

✅ Modify system files

Visual:

```text
                ROOT
                  │
     ┌────────────┼────────────┐
     │            │            │
   Users       Files      Services
```

---

# 👨‍💻 Normal Users

Regular users who use Linux daily.

Examples:

```text
rahul
priya
aman
```

Can:

✅ Create files

✅ Run applications

✅ Use the system

Cannot:

❌ Modify critical system files

❌ Install system-wide software without permission

---

# ⚙️ System Users

Special accounts created for services.

Examples:

```text
mysql
nginx
apache
www-data
postgres
```

These are not humans.

They allow services to run securely.

Visual:

```text
System Services

├── nginx
├── mysql
├── sshd
└── postgres
```

---

# 🏠 User Home Directory

Every user gets a personal workspace.

Example:

```text
/home/rahul
```

Structure:

```text
/home

├── rahul
├── priya
└── aman
```

Inside:

```text
/home/rahul

├── Documents
├── Downloads
├── Pictures
├── Projects
└── Desktop
```

Think of it as:

```text
User's Private Room
```

---

# 🖥️ Login Shell

A shell is the program used to interact with Linux.

Examples:

```text
/bin/bash
/bin/zsh
/bin/sh
/bin/fish
```

Visual:

```text
User
 │
 ▼
Shell
 │
 ▼
Linux Kernel
 │
 ▼
Hardware
```

---

# 📂 User Information Storage

Linux stores user information in:

```text
/etc/passwd
```

Example:

```text
rahul:x:1001:1001::/home/rahul:/bin/bash
```

Contains:

```text
Username
UID
GID
Home Directory
Shell
```

---

# 🔄 User Lifecycle

A user goes through several stages.

```text
Create User
     │
     ▼
Assign Password
     │
     ▼
Login
     │
     ▼
Work
     │
     ▼
Modify Account
     │
     ▼
Lock/Disable
     │
     ▼
Delete User
```

This is called the User Lifecycle.

---

# ➕ Creating Users

Administrators create users using:

```bash
useradd username
```

Example:

```bash
sudo useradd rahul
```

User created:

```text
rahul
```

But password is not yet set.

---

# 🔑 Setting User Password

Use:

```bash
sudo passwd rahul
```

Example:

```text
New Password:
Retype Password:
```

Linux stores an encrypted version.

Never plaintext.

---

# 🔍 Viewing Current User

Show current logged-in user:

```bash
whoami
```

Example:

```bash
rahul
```

---

# 📋 Viewing User Details

Use:

```bash
id rahul
```

Example:

```text
uid=1001(rahul)
gid=1001(rahul)
groups=1001(rahul)
```

Visual:

```text
User Identity

Username : rahul
UID      : 1001
GID      : 1001
```

---

# 🔄 Switching Users

Move from one user account to another.

Example:

```bash
su rahul
```

Or:

```bash
su - rahul
```

Visual:

```text
Current User
      │
      ▼
Switch User
      │
      ▼
Target User
```

---

# ✏️ Modifying Users

Change existing users.

Command:

```bash
usermod
```

Examples:

Change home directory:

```bash
sudo usermod -d /newhome/rahul rahul
```

Change shell:

```bash
sudo usermod -s /bin/zsh rahul
```

Rename user:

```bash
sudo usermod -l rahul_sharma rahul
```

---

# 🔒 Locking User Accounts

Temporarily disable login.

```bash
sudo passwd -l rahul
```

Visual:

```text
User Account

[ACTIVE]
    │
    ▼
Lock
    │
    ▼
[DISABLED]
```

---

# 🔓 Unlocking User Accounts

Enable account again.

```bash
sudo passwd -u rahul
```

---

# ❌ Deleting Users

Remove user account.

```bash
sudo userdel rahul
```

Delete account and home directory:

```bash
sudo userdel -r rahul
```

Visual:

```text
User
 │
 ▼
Delete
 │
 ▼
Removed from System
```

---

# 🔐 User Security Best Practices

Always:

✅ Use strong passwords

✅ Disable unused accounts

✅ Use sudo instead of root login

✅ Review user accounts regularly

✅ Lock inactive users

Avoid:

❌ Sharing accounts

❌ Using weak passwords

❌ Giving root access to everyone

---

# 🏢 Real-World Example

Company Server:

```text
Users

├── Developers
│   ├── Rahul
│   ├── Priya
│   └── Aman
│
├── QA Team
│   ├── Ravi
│   └── Simran
│
└── Admin Team
    ├── Admin1
    └── Admin2
```

Benefits:

```text
Security
Accountability
Access Control
Auditing
Easy Management
```

---

# 🧠 Interview Questions

### What is a user in Linux?

A user is an identity recognized by Linux that can log in and access resources.

---

### What is UID?

A unique numerical identifier assigned to every user.

---

### Which UID belongs to root?

```text
0
```

---

### Why does Linux use UID instead of usernames?

Because UIDs are unique and remain consistent even if usernames change.

---

### Where is user information stored?

```text
/etc/passwd
```

---

### How do you check your current user?

```bash
whoami
```

---

### How do you display user information?

```bash
id username
```

---

### How do you switch users?

```bash
su username
```

---

# 🎯 Summary

User Management is the process of:

```text
Creating Users
Managing Users
Modifying Users
Securing Users
Removing Users
```

Understanding users is the foundation of:

```text
Linux Administration
DevOps
Cloud Computing
Cybersecurity
System Engineering
```

---

# 📚 Next File

Continue with:

```text
group-management.md
```

You will learn:

✅ What groups are

✅ Primary and secondary groups

✅ GID concepts

✅ Group administration

✅ Real-world permission management
