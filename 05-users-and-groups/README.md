# 👥 Linux Users & Groups Mastery

> Learn how Linux identifies people, controls access, manages permissions, and secures systems.

---

# 🎯 What is This Module?

Imagine Linux is a huge school.

* Every student has an identity.
* Students belong to classes.
* Teachers have extra permissions.
* Principals can do everything.
* Some rooms are restricted.
* Some students can enter only specific areas.

Linux works exactly like this.

Every person who uses Linux is called a **User**.

Users can be organized into **Groups**.

Together, Users and Groups help Linux decide:

✅ Who can log in
✅ Who can read files
✅ Who can modify files
✅ Who can install software
✅ Who can access sensitive data
✅ Who can perform administrative tasks

This module teaches everything about Linux user and group management from beginner to system administrator level.

---

# 🏗️ Module Learning Path

```text
05-users-and-groups/
│
├── README.md                ← Start Here
│
├── user-management.md
├── group-management.md
│
├── passwd-file.md
├── shadow-file.md
├── group-file.md
│
├── useradd.md
├── usermod.md
├── userdel.md
│
├── passwd-command.md
│
├── sudo.md
├── sudoers-file.md
│
├── account-locking.md
├── user-environment.md
│
├── interview-questions.md
└── references.md
```

---

# 🌎 Why Users Exist

Without users:

```text
Anyone can access everything
Anyone can delete files
Anyone can change settings
Anyone can break the system
```

Linux solves this by creating identities.

Example:

```text
Laptop

├── Rahul
├── Priya
├── Admin
└── Guest
```

Each person gets:

* Username
* Password
* Home directory
* Permissions
* Settings

---

# 👤 What is a User?

A user is an identity recognized by Linux.

Example:

```bash
rahul
```

Linux internally stores:

```text
Username : rahul
UID      : 1001
Group    : developers
Home     : /home/rahul
Shell    : /bin/bash
```

Think of it as:

```text
School Student Card

Name      : Rahul
Roll No   : 1001
Class     : Developers
Locker    : /home/rahul
```

---

# 🆔 User ID (UID)

Linux does not really care about names.

Internally it uses numbers.

Example:

```text
rahul = UID 1001
priya = UID 1002
```

Linux sees:

```text
UID 1001 owns file.txt
```

instead of:

```text
rahul owns file.txt
```

---

# 👨‍💻 Types of Linux Users

## 1. Root User

The king of Linux.

```text
Username : root
UID      : 0
```

Can:

✅ Install software
✅ Delete files
✅ Create users
✅ Stop services
✅ Change passwords

Visual:

```text
             ROOT
               │
      ┌────────┼────────┐
      │        │        │
   Users    Files   Services
```

---

## 2. Normal Users

Regular people using Linux.

Example:

```text
rahul
priya
john
```

Can:

✅ Use applications
✅ Create files

Cannot:

❌ Modify system settings
❌ Change kernel files

---

## 3. System Users

Special accounts used by services.

Examples:

```text
mysql
nginx
www-data
daemon
```

These are not real humans.

Visual:

```text
System Users

├── nginx
├── mysql
├── sshd
└── postgres
```

---

# 👥 What is a Group?

A group is a collection of users.

Example:

```text
developers

├── rahul
├── priya
└── aman
```

Instead of assigning permissions to every user:

```text
rahul
priya
aman
```

Linux assigns permissions to:

```text
developers
```

Much easier.

---

# 🎨 Real World Example

Company:

```text
Developers Team

├── Rahul
├── Priya
└── Aman
```

Folder:

```text
/project
```

Permission:

```text
developers can access project folder
```

No need to configure each employee separately.

---

# 🔐 How Linux Decides Access

When a user accesses a file:

```text
Step 1
Check Owner

Step 2
Check Group

Step 3
Check Others
```

Visual:

```text
File

          file.txt
              │
     ┌────────┼────────┐
     │        │        │
  Owner    Group    Others
```

---

# 📂 Important Files You Will Learn

Linux stores user information in special files.

---

## /etc/passwd

Stores:

```text
Username
UID
GID
Home Directory
Shell
```

Example:

```text
rahul:x:1001:1001::/home/rahul:/bin/bash
```

Learn in:

```text
passwd-file.md
```

---

## /etc/shadow

Stores passwords securely.

Example:

```text
Encrypted Passwords
Password Expiry
Password Policies
```

Learn in:

```text
shadow-file.md
```

---

## /etc/group

Stores group information.

Example:

```text
developers:x:1001:rahul,priya
```

Learn in:

```text
group-file.md
```

---

# 🛠️ User Management Commands

Linux administrators frequently create and manage users.

---

## Create User

```bash
useradd rahul
```

Learn in:

```text
useradd.md
```

---

## Modify User

```bash
usermod
```

Learn in:

```text
usermod.md
```

---

## Delete User

```bash
userdel rahul
```

Learn in:

```text
userdel.md
```

---

## Change Password

```bash
passwd rahul
```

Learn in:

```text
passwd-command.md
```

---

# 🚀 What is sudo?

Normally:

```text
Normal User
   ↓
No Administrative Power
```

With sudo:

```text
Normal User
   ↓
Temporary Admin Access
```

Example:

```bash
sudo apt update
```

Visual:

```text
User
 │
 ▼
sudo
 │
 ▼
Root Privileges
 │
 ▼
System Operation
```

Learn in:

```text
sudo.md
sudoers-file.md
```

---

# 🔒 Account Security

Linux can:

✅ Lock users
✅ Disable logins
✅ Expire passwords
✅ Force password changes

Example:

```bash
passwd -l rahul
```

Learn in:

```text
account-locking.md
```

---

# 🏠 User Environment

Every user gets:

```text
Home Directory
Shell
Profile Files
Environment Variables
Aliases
```

Example:

```text
/home/rahul
```

Learn in:

```text
user-environment.md
```

---

# 🧠 Complete Learning Flow

```text
Start
 │
 ▼
What is a User?
 │
 ▼
What is a Group?
 │
 ▼
UID & GID
 │
 ▼
/etc/passwd
 │
 ▼
/etc/shadow
 │
 ▼
/etc/group
 │
 ▼
useradd
 │
 ▼
usermod
 │
 ▼
userdel
 │
 ▼
passwd
 │
 ▼
sudo
 │
 ▼
sudoers
 │
 ▼
Account Security
 │
 ▼
User Environment
 │
 ▼
Interview Questions
 │
 ▼
Linux User Management Mastered
```

---

# 🎯 Skills You Will Gain

After completing this module, you will be able to:

### Beginner

✅ Understand users and groups
✅ Log in and switch users
✅ Change passwords

### Intermediate

✅ Create users
✅ Create groups
✅ Assign permissions
✅ Manage home directories

### Advanced

✅ Configure sudo access
✅ Manage password policies
✅ Lock and unlock accounts
✅ Secure multi-user systems

### Professional

✅ Manage Linux servers
✅ Configure enterprise access control
✅ Troubleshoot authentication issues
✅ Administer production environments

---

# 💼 Real-World Usage

User and Group Management is used in:

### System Administration

```text
User Accounts
Permission Control
Access Management
```

### DevOps

```text
Jenkins Users
Docker Users
Kubernetes Access
```

### Cloud Computing

```text
AWS EC2
Azure VM
Google Cloud VM
```

### Cybersecurity

```text
Least Privilege
Access Control
Account Auditing
Identity Management
```

---

# 🏆 Final Goal

By the end of this module, you should understand:

```text
Who is using Linux?
How Linux identifies users?
How groups work?
How permissions are assigned?
How sudo works?
How Linux secures user accounts?
```

If you can confidently answer those questions, you have mastered the foundation of Linux User & Group Management.

---

# 📚 Next Step

Continue with:

```text
user-management.md
```

There you will learn:

✅ User lifecycle
✅ UID and GID concepts
✅ User creation process
✅ User account structure
✅ Real-world administration workflows
