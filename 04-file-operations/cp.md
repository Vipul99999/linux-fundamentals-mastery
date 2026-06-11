# cp Command (Copy Files and Directories)

# What is cp?

`cp` stands for:

```text
Copy
```

The `cp` command creates a duplicate (copy) of files and directories.

Think of it like a photocopy machine.

---

# Why Do We Need cp?

Imagine you have an important notebook.

```text
Original Notebook
```

You want:

```text
A Backup Copy
```

instead of risking damage to the original.

Linux works the same way.

---

# Real-Life Analogy

Imagine a school notebook.

```text
Notebook
│
└── Homework
```

You photocopy it.

Result:

```text
Original Notebook
│
└── Homework

Copied Notebook
│
└── Homework
```

Both exist independently.

Linux equivalent:

```bash
cp homework.txt backup.txt
```

---

# Relationship With Previous Commands

```text
mkdir
│
└── Create Directory

touch
│
└── Create File

cp
│
└── Duplicate File
```

---

# Syntax

```bash
cp source destination
```

Example:

```bash
cp notes.txt backup.txt
```

---

# Your First Copy

Create file:

```bash
touch notes.txt
```

Current state:

```text
Directory
│
└── notes.txt
```

Copy:

```bash
cp notes.txt backup.txt
```

Result:

```text
Directory
│
├── notes.txt
└── backup.txt
```

---

# Visual Understanding

Before:

```text
Directory
│
└── notes.txt
```

Command:

```bash
cp notes.txt backup.txt
```

After:

```text
Directory
│
├── notes.txt
│
└── backup.txt
```

Two separate files now exist.

---

# What Happens Internally?

When Linux executes:

```bash
cp notes.txt backup.txt
```

It performs:

```text
Read Source File
        │
        ▼
Copy Data
        │
        ▼
Create Destination File
        │
        ▼
Write Data
```

Result:

```text
New Independent File
```

---

# Visual Copy Flow

```text
notes.txt
     │
     ▼
Read Data
     │
     ▼
Duplicate Data
     │
     ▼
backup.txt
```

---

# Important Concept

After copying:

```text
notes.txt
backup.txt
```

are different files.

Changing one does NOT automatically change the other.

---

Visual:

```text
Before Edit

notes.txt
│
└── Hello

backup.txt
│
└── Hello
```

---

Edit:

```text
notes.txt
│
└── Hello World
```

Now:

```text
backup.txt
│
└── Hello
```

unchanged.

---

# Copy Into Another Directory

Filesystem:

```text
project
│
├── notes.txt
│
└── backup
```

Command:

```bash
cp notes.txt backup/
```

Result:

```text
project
│
├── notes.txt
│
└── backup
    │
    └── notes.txt
```

---

# Visual Movement

Before:

```text
project
│
├── notes.txt
│
└── backup
```

After:

```text
project
│
├── notes.txt
│
└── backup
    │
    └── notes.txt
```

Original remains.

Copy appears inside backup.

---

# Copy Multiple Files

Create:

```bash
touch file1 file2 file3
```

Directory:

```text
file1
file2
file3
backup/
```

Copy:

```bash
cp file1 file2 file3 backup/
```

Result:

```text
backup
│
├── file1
├── file2
└── file3
```

---

# Visual Example

```text
Before

Directory
│
├── file1
├── file2
├── file3
└── backup
```

After:

```text
Directory
│
├── file1
├── file2
├── file3
│
└── backup
    │
    ├── file1
    ├── file2
    └── file3
```

---

# Copy Entire Directories

Suppose:

```text
project
│
├── app.js
├── config.json
└── README.md
```

Attempt:

```bash
cp project backup
```

Error:

```text
omitting directory
```

Why?

Because directories need special handling.

---

# Recursive Copy

Use:

```bash
cp -r project backup
```

Result:

```text
backup
│
├── app.js
├── config.json
└── README.md
```

---

# Visual Recursive Copy

Before:

```text
project
│
├── app.js
├── config.json
└── README.md
```

After:

```text
project
│
├── app.js
├── config.json
└── README.md

backup
│
├── app.js
├── config.json
└── README.md
```

---

# Why -r Is Important

`-r` means:

```text
Recursive
```

Visual:

```text
Directory
│
├── File
│
└── Subdirectory
    │
    └── File
```

Linux copies:

```text
Everything
Inside
Everything
```

---

# Recursive Visualization

```text
project
│
├── file1
│
└── src
    │
    ├── main.js
    └── utils.js
```

Command:

```bash
cp -r project backup
```

Result:

```text
backup
│
├── file1
│
└── src
    │
    ├── main.js
    └── utils.js
```

---

# Interactive Copy Protection

Option:

```bash
cp -i source destination
```

If destination exists:

```text
Overwrite?
```

Linux asks before replacing.

---

Visual:

```text
Existing File
      │
      ▼
New Copy
      │
      ▼
Ask User First
```

---

# Force Copy

```bash
cp -f source destination
```

Linux overwrites without asking.

---

Visual:

```text
Old File
    │
    ▼
Deleted

New File
    │
    ▼
Written
```

---

# Preserve Metadata

Option:

```bash
cp -p file backup
```

Preserves:

```text
Permissions
Ownership
Timestamps
```

---

Visual:

```text
Original File
│
├── Data
├── Owner
├── Permissions
└── Time

Copied File
│
├── Data
├── Owner
├── Permissions
└── Time
```

---

# Verbose Mode

```bash
cp -v file backup
```

Output:

```text
'file' -> 'backup'
```

Useful for learning.

---

# Common Errors

## File Does Not Exist

Command:

```bash
cp abc.txt backup.txt
```

Error:

```text
No such file or directory
```

Visual:

```text
Looking For

abc.txt

Found?

No
```

---

## Forgetting -r

Directory:

```text
project
```

Command:

```bash
cp project backup
```

Error:

```text
omitting directory
```

Use:

```bash
cp -r project backup
```

---

## Permission Denied

Command:

```bash
cp notes.txt /root/
```

Error:

```text
Permission denied
```

---

# Real-World Use Cases

## Create Backups

```bash
cp report.txt report_backup.txt
```

---

## Backup Entire Projects

```bash
cp -r website website_backup
```

---

## Deployment

```bash
cp app.jar /opt/application/
```

---

## Configuration Safety

```bash
cp nginx.conf nginx.conf.backup
```

---

# Hands-On Practice Lab

Create:

```bash
mkdir practice

cd practice

touch notes.txt
```

Check:

```bash
ls
```

Output:

```text
notes.txt
```

---

Copy:

```bash
cp notes.txt backup.txt
```

Check:

```bash
ls
```

Output:

```text
notes.txt
backup.txt
```

---

Create:

```bash
mkdir backups
```

Copy:

```bash
cp notes.txt backups/
```

Check:

```bash
tree
```

Expected:

```text
.
├── notes.txt
├── backup.txt
└── backups
    └── notes.txt
```

---

Create project:

```bash
mkdir project

touch project/app.js
touch project/index.html
```

Copy:

```bash
cp -r project project_backup
```

Result:

```text
project_backup
│
├── app.js
└── index.html
```

---

# Memory Trick

Think:

```text
Photocopy Machine
        │
        ▼
Original Paper
        │
        ▼
Duplicate Paper
```

Linux:

```text
Original File
        │
        ▼
cp
        │
        ▼
Copied File
```

---

# Visual Summary

```text
touch
│
└── Create New Empty File

cp
│
└── Duplicate Existing File

mv
│
└── Move/Rename File

rm
│
└── Delete File
```

---

# Interview Questions

## What does cp stand for?

```text
Copy
```

---

## Does cp remove the original file?

No.

The original remains unchanged.

---

## What does cp -r do?

Recursively copies directories and their contents.

---

## Difference Between cp and mv?

```text
cp
│
└── Creates Duplicate

mv
│
└── Moves Original
```

---

## Why use cp -p?

To preserve:

```text
Permissions
Ownership
Timestamps
```

---

## What happens if destination exists?

Default behavior:

```text
Overwrite
```

Use:

```bash
cp -i
```

for confirmation.

---

# Quick Summary

```text
Command:
cp

Purpose:
Copy Files And Directories

Important Options:

-r  Recursive Copy
-i  Interactive
-f  Force
-p  Preserve Metadata
-v  Verbose

Remember:

touch → Create

cp → Duplicate

mv → Move

rm → Delete
```
