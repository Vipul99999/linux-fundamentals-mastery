# cd Command (Change Directory)

# What is cd?

`cd` stands for:

```text
Change Directory
```

The `cd` command is used to move from one directory to another.

Think of it like walking from one room to another room in a house.

---

# Why Do We Need cd?

Imagine you are standing in your house.

```text
House
│
├── Bedroom
├── Kitchen
├── Living Room
└── Study Room
```

You can see all rooms.

But seeing them is not enough.

You must be able to move into them.

Linux works exactly the same way.

`cd` allows you to move around the filesystem.

---

# Real-Life Analogy

Imagine a school building.

```text
School
│
├── Class 1
├── Class 2
├── Library
└── Laboratory
```

Current location:

```text
Class 1
```

To go to the Library:

```text
Walk to Library
```

Linux equivalent:

```bash
cd Library
```

---

# Relationship Between pwd, ls and cd

Think of these commands as a team.

```text
pwd → Where am I?

ls → What is here?

cd → Go somewhere else
```

Visual:

```text
You
 │
 ▼
pwd
 │
 ▼
Current Location

 │
 ▼
ls
 │
 ▼
Available Places

 │
 ▼
cd
 │
 ▼
Move There
```

---

# Syntax

```bash
cd directory_name
```

Example:

```bash
cd Documents
```

---

# Understanding Current Directory

Suppose:

```bash
pwd
```

Output:

```text
/home/vip
```

Filesystem:

```text
/home/vip
│
├── Documents
├── Downloads
├── Pictures
└── Music
```

Move:

```bash
cd Documents
```

Now:

```bash
pwd
```

Output:

```text
/ home / vip / Documents
```

---

# Visual Walkthrough

Before:

```text
/home
│
└── vip
     │
     ├── Documents
     ├── Downloads
     └── Music

You are here
     ▲
     │
    vip
```

Command:

```bash
cd Documents
```

After:

```text
/home
│
└── vip
     │
     ├── Documents
     │      ▲
     │      │
     │   You are here
     │
     ├── Downloads
     └── Music
```

---

# How cd Works Internally

Many beginners think:

```text
cd opens a folder
```

Not exactly.

Linux changes the process's:

```text
Current Working Directory (CWD)
```

Visual:

```text
Before

Shell
 │
 ▼
Current Directory
/home/vip

After

cd Documents

Shell
 │
 ▼
Current Directory
/home/vip/Documents
```

No files move.

No folders move.

Only your location changes.

---

# Absolute Path Navigation

Absolute path starts from root:

```text
/
```

Example:

```bash
cd /home/vip/Documents
```

Visual:

```text
/
│
└── home
     │
     └── vip
          │
          └── Documents
```

Linux follows the entire path.

---

# Relative Path Navigation

Relative path starts from current location.

Current:

```bash
pwd
```

Output:

```text
/home/vip
```

Move:

```bash
cd Documents
```

Linux understands:

```text
/home/vip/Documents
```

because it already knows where you are.

---

# Absolute vs Relative Path

## Absolute

```bash
cd /home/vip/Documents
```

Starts from root.

---

## Relative

```bash
cd Documents
```

Starts from current location.

---

Visual:

```text
Absolute Path

/
│
└── home
     │
     └── vip
          │
          └── Documents

Relative Path

Current
│
└── Documents
```

---

# Moving Back One Directory

Special symbol:

```text
..
```

means:

```text
Parent Directory
```

Example:

Current:

```text
/home/vip/Documents
```

Command:

```bash
cd ..
```

Result:

```text
/home/vip
```

---

Visual

Before:

```text
home
│
└── vip
     │
     └── Documents
           ▲
           │
       You Here
```

After:

```bash
cd ..
```

```text
home
│
└── vip
     ▲
     │
 You Here
```

---

# Moving Back Multiple Levels

Current:

```text
/home/vip/Documents/Projects
```

Command:

```bash
cd ../..
```

Visual:

```text
home
│
└── vip
     │
     └── Documents
           │
           └── Projects
```

Move:

```text
Projects
   ▲
   │
   └── ..
        ▲
        │
        └── ..
```

Result:

```text
/home/vip
```

---

# Current Directory Symbol

Special symbol:

```text
.
```

means:

```text
Current Directory
```

Example:

```bash
cd .
```

Nothing changes.

Current location remains same.

---

# Home Directory Shortcut

Symbol:

```text
~
```

means:

```text
Your Home Directory
```

Example:

```bash
cd ~
```

Result:

```text
/home/vip
```

---

Visual:

```text
~
│
▼
Home Directory
```

---

# Move Directly Home

Shortcut:

```bash
cd
```

No arguments.

Linux automatically takes you home.

Example:

Current:

```text
/home/vip/Documents/Projects
```

Run:

```bash
cd
```

Result:

```text
/home/vip
```

---

# Previous Directory Shortcut

Special symbol:

```text
-
```

Move back to previous location.

Example:

Current:

```text
/home/vip
```

Move:

```bash
cd Documents
```

Now:

```text
/home/vip/Documents
```

Run:

```bash
cd -
```

Result:

```text
/home/vip
```

---

Visual:

```text
Location A
     │
     ▼
Location B
     │
cd -
     │
     ▼
Location A
```

---

# Common Navigation Patterns

## Go Home

```bash
cd
```

---

## Go Root

```bash
cd /
```

---

## Go Parent

```bash
cd ..
```

---

## Go Previous Directory

```bash
cd -
```

---

## Go Specific Directory

```bash
cd Documents
```

---

# Common Errors

## Directory Does Not Exist

```bash
cd abc
```

Error:

```text
No such file or directory
```

---

Visual:

```text
Current Directory
│
├── Documents
├── Downloads
└── Pictures
```

Trying:

```bash
cd Movies
```

Fails because Movies doesn't exist.

---

## Permission Denied

```bash
cd protected_folder
```

Error:

```text
Permission denied
```

You do not have access.

---

# Real World Use Cases

## Software Development

Move into project:

```bash
cd my-project
```

Run application:

```bash
npm start
```

---

## System Administration

Move to configuration files:

```bash
cd /etc
```

---

## Log Analysis

```bash
cd /var/log
```

Inspect logs.

---

## DevOps

```bash
cd /opt/application
```

Deploy services.

---

# Hands-On Practice Lab

Create:

```bash
mkdir -p learning/linux/practice
```

Navigate:

```bash
cd learning
pwd
```

Expected:

```text
.../learning
```

---

Move:

```bash
cd linux
pwd
```

Expected:

```text
.../learning/linux
```

---

Move:

```bash
cd practice
pwd
```

Expected:

```text
.../learning/linux/practice
```

---

Move back:

```bash
cd ..
pwd
```

Expected:

```text
.../learning/linux
```

---

Move home:

```bash
cd
pwd
```

Expected:

```text
/home/username
```

---

# Memory Trick

Think:

```text
pwd
=
Where am I?

ls
=
What is here?

cd
=
Go there
```

---

Visual Memory Map

```text
Linux Navigation

        pwd
         │
         ▼
  Where am I?

         │
         ▼
         ls
         │
         ▼
  What is here?

         │
         ▼
         cd
         │
         ▼
  Move there
```

---

# Interview Questions

## What does cd stand for?

```text
Change Directory
```

---

## What is the purpose of cd?

To change the current working directory.

---

## Difference between absolute and relative paths?

Absolute path starts from root (`/`).

Relative path starts from current directory.

---

## What does `cd ..` do?

Moves to parent directory.

---

## What does `cd ~` do?

Moves to home directory.

---

## What does `cd -` do?

Moves to previous directory.

---

# Quick Summary

```text
Command:
cd

Purpose:
Move between directories

Most Common Usage:

cd Documents
cd ..
cd ~
cd /
cd -

Remember:

pwd → Where am I?
ls  → What is here?
cd  → Go there
```
