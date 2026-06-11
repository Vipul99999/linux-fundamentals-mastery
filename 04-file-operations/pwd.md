# pwd Command (Print Working Directory)

# What is `pwd`?

`pwd` stands for:

```text
Print Working Directory
```

It displays the **absolute path** of the directory you are currently inside.

Think of it like a GPS for your terminal.

When you ask Linux:

```bash
pwd
```

Linux replies:

```text
You are currently here.
```

---

# Why Do We Need pwd?

Imagine you are inside a huge building.

```text
Building
│
├── Floor 1
├── Floor 2
├── Floor 3
├── Floor 4
└── Floor 5
```

Someone asks:

> "Where are you?"

You must know your location before moving anywhere.

Linux works exactly the same way.

The filesystem may contain millions of files and directories.

Before:

* Creating files
* Copying files
* Moving files
* Deleting files
* Running programs

You should know:

```text
Where am I?
```

That is exactly what `pwd` tells us.

---

# Real Life Analogy

Imagine Google Maps.

```text
India
 └── Uttar Pradesh
      └── Kanpur
           └── Home
                └── Bedroom
```

Your current location:

```text
Bedroom
```

Google Maps shows:

```text
India → Uttar Pradesh → Kanpur → Home → Bedroom
```

Similarly Linux shows:

```text
/home/vip/projects
```

---

# Syntax

```bash
pwd
```

---

# Basic Example

Command:

```bash
pwd
```

Output:

```text
/home/vip
```

Meaning:

```text
Root
 │
 └── home
      │
      └── vip
```

You are currently inside:

```text
vip
```

directory.

---

# Visual Understanding

Suppose filesystem looks like:

```text
/
│
├── bin
├── boot
├── dev
├── etc
├── home
│    │
│    └── vip
│         │
│         ├── Documents
│         ├── Downloads
│         └── Projects
│
└── usr
```

Current location:

```text
Projects
```

Visual:

```text
/
│
└── home
     │
     └── vip
          │
          └── Projects
               ▲
               │
          You are here
```

Running:

```bash
pwd
```

Output:

```text
/home/vip/Projects
```

---

# What Does "Working Directory" Mean?

Every process in Linux has a current location.

This location is called:

```text
Current Working Directory (CWD)
```

Example:

```bash
cd Documents
```

Now:

```bash
pwd
```

Output:

```text
/home/vip/Documents
```

Linux remembers:

```text
Current Working Directory
=
/home/vip/Documents
```

---

# Absolute Path vs Relative Path

Understanding this is extremely important.

---

## Absolute Path

Starts from root directory.

Example:

```text
/home/vip/Documents
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

Absolute paths always begin with:

```text
/
```

---

## Relative Path

Starts from current location.

Suppose:

```bash
pwd
```

returns:

```text
/home/vip
```

You can access:

```text
Documents
```

instead of:

```text
/home/vip/Documents
```

because Linux already knows where you are.

---

# How pwd Works Internally

When you run:

```bash
pwd
```

Linux does not search the entire filesystem.

The shell already stores:

```text
Current Working Directory
```

for the running process.

Process structure:

```text
Shell Process
│
├── PID
├── Environment
├── Variables
└── Current Working Directory
```

When `pwd` executes:

```text
Kernel
   │
   ▼
Current Working Directory
   │
   ▼
Print Path
```

This makes `pwd` extremely fast.

---

# pwd Flow Diagram

```text
User
 │
 │ pwd
 ▼
Shell
 │
 ▼
Kernel
 │
 ▼
Current Working Directory
 │
 ▼
Print Location
```

Output:

```text
/home/vip/projects
```

---

# Root Directory Example

Move to root:

```bash
cd /
```

Check:

```bash
pwd
```

Output:

```text
/
```

Visual:

```text
/
▲
│
You are here
```

---

# Home Directory Example

Move home:

```bash
cd ~
```

Check:

```bash
pwd
```

Output:

```text
/home/vip
```

Visual:

```text
/home
    │
    └── vip
         ▲
         │
      You
```

---

# pwd After Directory Changes

Initial:

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

Check:

```bash
pwd
```

Output:

```text
/home/vip/Documents
```

Move again:

```bash
cd Projects
```

Output:

```text
/home/vip/Documents/Projects
```

Visual:

```text
/home
 │
 └── vip
      │
      └── Documents
            │
            └── Projects
                 ▲
                 │
              Current
```

---

# Physical Path vs Logical Path

Linux supports symbolic links.

Filesystem:

```text
/home/vip/project
```

Symbolic link:

```text
shortcut -> /home/vip/project
```

Move:

```bash
cd shortcut
```

Now:

```bash
pwd
```

May show:

```text
/home/vip/shortcut
```

Logical path.

---

Physical path:

```bash
pwd -P
```

Output:

```text
/home/vip/project
```

Actual location.

---

# pwd Options

## Logical Path (Default)

```bash
pwd -L
```

Output:

```text
/home/vip/shortcut
```

---

## Physical Path

```bash
pwd -P
```

Output:

```text
/home/vip/project
```

---

# Real World Use Cases

## Before Deleting Files

Always verify location.

```bash
pwd
rm *
```

Wrong directory can destroy important files.

---

## Running Scripts

```bash
pwd
```

Check project root.

```bash
./build.sh
```

---

## DevOps

Before deployment:

```bash
pwd
```

Verify:

```text
/var/www/application
```

not

```text
/home/user/test
```

---

## System Administration

Before editing configuration:

```bash
pwd
```

Verify:

```text
/etc/nginx
```

---

# Common Beginner Mistakes

---

## Mistake 1

Deleting files in wrong directory.

```bash
rm *
```

without checking:

```bash
pwd
```

---

## Mistake 2

Running script from wrong location.

```bash
./run.sh
```

Script fails because current directory is incorrect.

---

## Mistake 3

Confusing Relative Paths

Current location:

```text
/home/vip
```

Trying:

```bash
cat notes.txt
```

But file exists in:

```text
/home/vip/Documents
```

Check location:

```bash
pwd
```

---

# Interview Questions

## Q1

What does pwd stand for?

Answer:

```text
Print Working Directory
```

---

## Q2

What is a Working Directory?

Answer:

```text
The current directory in which a process is operating.
```

---

## Q3

Difference between pwd -L and pwd -P?

Answer:

```text
-L → Logical path
-P → Physical path
```

---

## Q4

Why is pwd important?

Answer:

```text
It helps identify the current filesystem location before performing operations.
```

---

# Hands-On Practice

Create structure:

```bash
mkdir -p learning/linux/files
```

Move:

```bash
cd learning
pwd
```

Expected:

```text
.../learning
```

Move:

```bash
cd linux
pwd
```

Expected:

```text
.../learning/linux
```

Move:

```bash
cd files
pwd
```

Expected:

```text
.../learning/linux/files
```

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

# Quick Summary

```text
Command:
pwd

Meaning:
Print Working Directory

Purpose:
Shows current location

Most Common Usage:
pwd

Physical Path:
pwd -P

Logical Path:
pwd -L
```

---

# Memory Trick

Think:

```text
GPS → Location → Where am I?
```

Linux version:

```text
pwd → Current Location → Where am I?
```

Whenever you feel lost in Linux:

```bash
pwd
```

asks Linux:

```text
"Where am I right now?"
```
