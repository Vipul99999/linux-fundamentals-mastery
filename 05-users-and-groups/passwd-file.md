# 📄 Understanding `/etc/passwd` in Linux

> Learn the most important user database file in Linux: **`/etc/passwd`**. By the end of this guide, you'll understand every field, how Linux identifies users, how logins work, and how this file connects to the entire user management system.

---

# 🎯 What is `/etc/passwd`?

Imagine a school office maintains a register of all students.

Example:

```text
Name      Roll No    Class
Rahul     1001       A
Priya     1002       A
Aman      1003       B
```

Linux maintains a similar registry for users.

That registry is:

```text
/etc/passwd
```

It contains information about every user known to the system.

---

# 🧠 Important Clarification

Many beginners think:

```text
/etc/passwd = Password File
```

Historically this was true.

Modern Linux systems store passwords elsewhere:

```text
/etc/shadow
```

Today, `/etc/passwd` mainly stores:

```text
Username
UID
GID
User Information
Home Directory
Login Shell
```

Not actual passwords.

---

# 🌍 Why Linux Needs `/etc/passwd`

Whenever someone logs in:

```text
Username → Rahul
```

Linux must determine:

```text
Who is Rahul?
What is Rahul's UID?
Which group belongs to Rahul?
Where is Rahul's home directory?
Which shell should start?
```

Linux finds all this information in:

```text
/etc/passwd
```

---

# 🏗️ Where Is It Located?

Location:

```text
/etc/passwd
```

Verify:

```bash
ls -l /etc/passwd
```

Example:

```text
-rw-r--r-- 1 root root 2500 Jun 12 12:00 /etc/passwd
```

Notice:

```text
Readable by everyone
Writable only by root
```

---

# 🎨 Why Everyone Can Read It

Linux needs to map:

```text
UID → Username
Username → UID
```

for many operations.

Example:

```bash
ls -l
```

Output:

```text
-rw-r--r-- rahul developers file.txt
```

Internally Linux stores:

```text
UID 1001
GID 1001
```

but displays:

```text
rahul
developers
```

using information from `/etc/passwd`.

---

# 📋 Viewing the File

Show contents:

```bash
cat /etc/passwd
```

Example:

```text
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
rahul:x:1001:1001:Rahul Sharma:/home/rahul:/bin/bash
```

Each line represents one user.

---

# 🏗️ Structure of an Entry

General format:

```text
username:password:UID:GID:GECOS:home:shell
```

Visual:

```text
┌─────────┬─────────┬──────┬──────┬─────────┬──────────────┬─────────────┐
│ username│ password│ UID  │ GID  │ GECOS   │ home         │ shell       │
└─────────┴─────────┴──────┴──────┴─────────┴──────────────┴─────────────┘
```

Example:

```text
rahul:x:1001:1001:Rahul Sharma:/home/rahul:/bin/bash
```

---

# 🔍 Field 1 — Username

Example:

```text
rahul
```

This is the login name.

Visual:

```text
User Login

rahul
  │
  ▼
Linux searches /etc/passwd
```

Commands:

```bash
whoami
```

Output:

```text
rahul
```

---

# 🔍 Field 2 — Password Placeholder

Example:

```text
x
```

Modern systems use:

```text
x
```

which means:

```text
Actual password stored in /etc/shadow
```

Historical systems stored encrypted passwords directly here.

---

# 🔍 Field 3 — UID (User ID)

Example:

```text
1001
```

Linux internally identifies users using UID.

Visual:

```text
rahul
  │
  ▼
UID 1001
```

Linux primarily works with UIDs, not usernames.

---

# Common UID Ranges

```text
0           → Root
1–999       → System Accounts
1000+       → Human Users
```

Visual:

```text
UID Space

0
│
├── Root

1-999
│
├── System Users

1000+
│
└── Normal Users
```

---

# 🔍 Field 4 — GID (Group ID)

Example:

```text
1001
```

Represents the user's primary group.

Visual:

```text
Rahul
  │
  ▼
Developers
  │
  ▼
GID 1001
```

Linux uses GID to determine group permissions.

---

# 🔍 Field 5 — GECOS

Example:

```text
Rahul Sharma
```

Stores descriptive information.

Can contain:

```text
Full Name
Office Number
Phone Number
Department
Comments
```

Example:

```text
Rahul Sharma,IT Department
```

---

# Viewing GECOS Information

Use:

```bash
finger rahul
```

or

```bash
getent passwd rahul
```

---

# 🔍 Field 6 — Home Directory

Example:

```text
/home/rahul
```

This is the user's personal workspace.

Visual:

```text
Linux System

/home
│
├── rahul
├── priya
└── aman
```

After login:

```text
User lands here
```

---

# 🔍 Field 7 — Login Shell

Example:

```text
/bin/bash
```

Determines which shell starts after login.

Common shells:

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
Linux
```

---

# 🏗️ Complete Example Breakdown

Entry:

```text
rahul:x:1001:1001:Rahul Sharma:/home/rahul:/bin/bash
```

Meaning:

```text
Username      : rahul
Password      : x
UID           : 1001
GID           : 1001
Full Name     : Rahul Sharma
Home Directory: /home/rahul
Shell         : /bin/bash
```

---

# 👑 Understanding the Root Entry

Root user:

```text
root:x:0:0:root:/root:/bin/bash
```

Breakdown:

```text
Username : root
UID      : 0
GID      : 0
Home     : /root
Shell    : /bin/bash
```

Visual:

```text
ROOT
 │
 ├── All Files
 ├── All Users
 ├── All Services
 └── Full Control
```

---

# ⚙️ System Accounts

Example:

```text
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
```

Notice:

```text
Shell = /usr/sbin/nologin
```

Meaning:

```text
Cannot login interactively
```

Used by services.

Examples:

```text
nginx
mysql
postgres
sshd
```

---

# 🚫 What Is `/usr/sbin/nologin`?

Some accounts exist only for services.

Example:

```text
mysql
```

Humans should never log in as mysql.

Therefore:

```text
Shell = /usr/sbin/nologin
```

Visual:

```text
Login Attempt
      │
      ▼
nologin
      │
      ▼
Access Denied
```

---

# 🔎 Searching for Users

Find a user:

```bash
grep rahul /etc/passwd
```

Output:

```text
rahul:x:1001:1001:Rahul Sharma:/home/rahul:/bin/bash
```

---

# 📋 Querying with getent

Preferred modern method:

```bash
getent passwd rahul
```

Why?

Because it works with:

```text
Local Users
LDAP
NIS
Active Directory
SSSD
```

not just `/etc/passwd`.

---

# 🔄 How Login Uses `/etc/passwd`

Visual Flow:

```text
User Login
     │
     ▼
Enter Username
     │
     ▼
Search /etc/passwd
     │
     ▼
Find UID
Find GID
Find Home
Find Shell
     │
     ▼
Check Password
(/etc/shadow)
     │
     ▼
Login Success
```

---

# 🔐 Security Considerations

Safe:

```text
Readable by everyone
```

because:

```text
No passwords stored here
```

Dangerous:

```text
Writable by everyone
```

which is why only root can modify it.

---

# 🚨 Common Mistakes

## Editing Directly

Bad:

```bash
nano /etc/passwd
```

Possible:

```text
Broken user database
Login failures
System issues
```

Use:

```bash
vipw
```

instead.

---

# Why vipw?

```bash
sudo vipw
```

Benefits:

```text
Locks file
Prevents corruption
Performs validation
```

Used by administrators.

---

# 🏢 Real-World Example

Corporate Linux Server:

```text
UID 1001 → Rahul
UID 1002 → Priya
UID 1003 → Aman

Groups

1001 → Developers
1002 → QA
1003 → Admins
```

When Rahul logs in:

```text
Linux checks /etc/passwd
Finds UID 1001
Loads /home/rahul
Starts /bin/bash
```

---

# ⚡ Useful Commands Cheat Sheet

Show file:

```bash
cat /etc/passwd
```

---

Search user:

```bash
grep username /etc/passwd
```

---

Lookup user:

```bash
getent passwd username
```

---

Show current user:

```bash
whoami
```

---

Show user details:

```bash
id username
```

---

Safely edit:

```bash
sudo vipw
```

---

Count users:

```bash
wc -l /etc/passwd
```

---

List usernames only:

```bash
cut -d: -f1 /etc/passwd
```

---

# 🧠 Interview Questions

### What is `/etc/passwd`?

A user database file that stores account information.

---

### Does `/etc/passwd` contain passwords?

Modern Linux systems:

```text
No
```

Passwords are stored in:

```text
/etc/shadow
```

---

### What does the "x" field mean?

Password is stored in `/etc/shadow`.

---

### What is UID?

User Identifier.

---

### What is GID?

Group Identifier.

---

### What command safely edits `/etc/passwd`?

```bash
vipw
```

---

### What file contains actual password hashes?

```text
/etc/shadow
```

---

### What does `/usr/sbin/nologin` do?

Prevents interactive login.

---

### Why is `/etc/passwd` world-readable?

Linux needs user-to-UID mappings for many operations.

---

# 🎯 Summary

`/etc/passwd` is the central user database of Linux.

It stores:

```text
Username
UID
GID
User Information
Home Directory
Login Shell
```

It helps Linux:

```text
Identify Users
Map UIDs
Load Home Directories
Start Login Shells
Support Authentication
```

Understanding `/etc/passwd` is essential for:

```text
Linux Administration
DevOps
Cloud Engineering
Cybersecurity
System Troubleshooting
```

---

# 📚 Next File

Continue with:

```text
shadow-file.md
```

You will learn:

✅ How Linux stores passwords

✅ Password hashes

✅ Password expiration policies

✅ Account aging

✅ Security mechanisms behind authentication
