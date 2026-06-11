# mkdir Command (Make Directory)

# What is mkdir?

`mkdir` stands for:

```text
Make Directory
```

The `mkdir` command is used to create new directories (folders) in Linux.

Think of a directory as a container that can hold:

* Files
* Other directories

When you use `mkdir`, you are creating a new place to organize information.

---

# Why Do We Need Directories?

Imagine a school.

Without classrooms:

```text
School
│
├── Book1
├── Book2
├── Book3
├── Book4
├── Book5
├── Book6
├── Book7
└── Book8
```

Everything becomes messy.

Instead:

```text
School
│
├── Class1
│   ├── Book1
│   └── Book2
│
├── Class2
│   ├── Book3
│   └── Book4
│
└── Library
    ├── Book5
    └── Book6
```

Directories help organize data.

---

# Real-Life Analogy

Imagine a house.

```text
House
│
├── Bedroom
├── Kitchen
├── Bathroom
└── Study Room
```

Each room stores different things.

Linux directories work the same way.

```text
Project
│
├── src
├── docs
├── images
└── tests
```

Each directory has a purpose.

---

# Syntax

```bash
mkdir directory_name
```

Example:

```bash
mkdir Documents
```

Creates:

```text
Documents/
```

---

# Your First Directory

Command:

```bash
mkdir school
```

Check:

```bash
ls
```

Output:

```text
school
```

Visual:

Before:

```text
Current Directory
│
├── notes.txt
└── report.txt
```

After:

```text
Current Directory
│
├── notes.txt
├── report.txt
└── school
```

---

# Understanding What Happens Internally

When you run:

```bash
mkdir school
```

Linux does not create a visible folder icon.

Internally:

```text
Filesystem
    │
    ▼
Create New Directory Entry
    │
    ▼
Allocate Metadata
    │
    ▼
Assign Permissions
    │
    ▼
Update Parent Directory
```

Result:

```text
school/
```

appears in the filesystem.

---

# Creating Multiple Directories

Instead of:

```bash
mkdir docs
mkdir images
mkdir videos
```

You can do:

```bash
mkdir docs images videos
```

Result:

```text
Current Directory
│
├── docs
├── images
└── videos
```

---

# Visual Example

Command:

```bash
mkdir apple banana mango
```

Result:

```text
Current Directory
│
├── apple
├── banana
└── mango
```

Three directories created instantly.

---

# Nested Directories

Sometimes we need directories inside directories.

Example:

```text
project
└── src
    └── components
```

Command:

```bash
mkdir -p project/src/components
```

Result:

```text
project
└── src
    └── components
```

Linux creates everything automatically.

---

# Why -p is Important

Without `-p`:

```bash
mkdir project/src/components
```

Error:

```text
No such file or directory
```

Because:

```text
project
```

does not exist yet.

With:

```bash
mkdir -p project/src/components
```

Linux creates:

```text
project
└── src
    └── components
```

in one command.

---

# Visualizing -p

Without `-p`

```text
Need:
project
│
└── src
    │
    └── components

But project doesn't exist
```

Failure.

---

With `-p`

```text
Create project
      │
      ▼
Create src
      │
      ▼
Create components
```

Success.

---

# Creating Project Structures

Real-world example:

```bash
mkdir -p myapp/src
mkdir -p myapp/tests
mkdir -p myapp/docs
```

Result:

```text
myapp
│
├── src
├── tests
└── docs
```

---

# Creating Deep Structures

Command:

```bash
mkdir -p company/hr/employees
```

Result:

```text
company
│
└── hr
    │
    └── employees
```

---

# Directory Names with Spaces

Suppose directory name contains spaces.

Wrong:

```bash
mkdir My Project
```

Creates:

```text
My
Project
```

Two directories.

---

Correct:

```bash
mkdir "My Project"
```

Result:

```text
My Project
```

One directory.

---

# Escape Spaces

Alternative:

```bash
mkdir My\ Project
```

Also creates:

```text
My Project
```

---

# Creating Hidden Directories

Linux hides names starting with `.`

Example:

```bash
mkdir .secret
```

Directory:

```text
.secret
```

Normal listing:

```bash
ls
```

May not show it.

Use:

```bash
ls -a
```

Output:

```text
.secret
```

---

# Checking Directory Creation

Create:

```bash
mkdir practice
```

Verify:

```bash
ls
```

or

```bash
tree
```

Visual:

```text
Current Directory
│
└── practice
```

---

# Common mkdir Options

## Create Parents

```bash
mkdir -p project/src
```

---

## Verbose Mode

```bash
mkdir -v test
```

Output:

```text
created directory 'test'
```

---

## Create Multiple

```bash
mkdir a b c
```

---

# Common Errors

## Directory Already Exists

Command:

```bash
mkdir docs
```

Again:

```bash
mkdir docs
```

Error:

```text
File exists
```

---

Visual:

```text
Current Directory
│
└── docs
```

Linux cannot create the same directory twice.

---

# Fix Using -p

```bash
mkdir -p docs
```

No error.

Linux simply continues.

---

# Permission Denied

Example:

```bash
mkdir /root/test
```

Output:

```text
Permission denied
```

Reason:

```text
You do not own that location.
```

---

# Real-World Use Cases

## Software Development

Create project structure:

```bash
mkdir src tests docs
```

---

## Web Development

```bash
mkdir css js images
```

Result:

```text
website
│
├── css
├── js
└── images
```

---

## DevOps

```bash
mkdir backups
```

Store backup files.

---

## System Administration

```bash
mkdir logs
```

Store log files.

---

# Hands-On Practice Lab

Create:

```bash
mkdir school
```

Check:

```bash
ls
```

---

Create multiple:

```bash
mkdir math science english
```

Check:

```bash
ls
```

---

Create nested:

```bash
mkdir -p company/hr/employees
```

Check:

```bash
tree
```

Expected:

```text
company
└── hr
    └── employees
```

---

Create hidden directory:

```bash
mkdir .private
```

Check:

```bash
ls -a
```

---

# Memory Trick

Think:

```text
House Builder
      │
      ▼
Create Room
      │
      ▼
mkdir
```

Whenever you need a new place:

```bash
mkdir
```

creates it.

---

# Visual Summary

```text
pwd
│
├── Where am I?
│
ls
│
├── What is here?
│
cd
│
├── Go somewhere
│
mkdir
│
└── Create somewhere
```

---

# Interview Questions

## What does mkdir stand for?

```text
Make Directory
```

---

## What is a directory?

A special filesystem object used to organize files and other directories.

---

## What does mkdir -p do?

Creates parent directories automatically.

---

## Difference between mkdir and mkdir -p?

```text
mkdir
│
└── Parent must exist

mkdir -p
│
└── Creates missing parents automatically
```

---

## How do you create multiple directories?

```bash
mkdir docs images videos
```

---

## How do you create a hidden directory?

```bash
mkdir .secret
```

---

# Quick Summary

```text
Command:
mkdir

Purpose:
Create directories

Most Common Usage:

mkdir docs

mkdir docs images videos

mkdir -p project/src/components

Important Option:

-p
Create parent directories

Remember:

pwd   → Where am I?

ls    → What is here?

cd    → Go there

mkdir → Create a new place
```
