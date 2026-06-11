# visuals.md

# Linux File Operations Visual Atlas

> One file to visualize everything learned in:
>
> ```text
> 04-file-operations/
> ```

---

# Big Picture

```text
Linux File Operations

                     FILESYSTEM

                           │
                           ▼

        ┌────────────────────────────────────┐
        │                                    │
        ▼                                    ▼

   Navigation                         Manipulation

        │                                    │

   pwd  ls  cd                  touch cp mv rm mkdir

        │                                    │
        └────────────────┬───────────────────┘
                         │
                         ▼

                    Inspection

                         │

           cat less head tail stat file

                         │
                         ▼

                     Searching

                         │

             find locate tree wildcards

                         │
                         ▼

                    Data Flow

                         │

                 Redirection

                         │

                      Pipes
```

---

# Filesystem Navigation

```text
/

├── home
│
├── etc
│
├── var
│
├── usr
│
└── tmp
```

Current location:

```text
/

└── home
    │
    └── vip
        │
        └── projects   ← You Are Here
```

Command:

```bash
pwd
```

Output:

```text
/home/vip/projects
```

---

# Directory Movement

```text
/home/vip

├── docs
├── projects
└── downloads
```

Current:

```text
/home/vip
```

Move:

```bash
cd projects
```

Result:

```text
/home/vip/projects
```

Visual:

```text
home
 │
 ▼
vip
 │
 ▼
projects
```

---

# File Creation

```bash
touch notes.txt
```

Visual:

```text
Before

Directory
│
└── (empty)


After

Directory
│
└── notes.txt
```

---

# Directory Creation

```bash
mkdir project
```

Visual:

```text
Before

workspace
│


After

workspace
│
└── project
```

---

# Copy Operation

```bash
cp report.txt backup/
```

Visual:

```text
Before

report.txt


After

report.txt
backup/report.txt
```

---

# Move Operation

```bash
mv report.txt archive/
```

Visual:

```text
Before

report.txt


After

archive/report.txt
```

---

# Rename Operation

```bash
mv old.txt new.txt
```

Visual:

```text
Before

old.txt


After

new.txt
```

---

# Delete Operation

```bash
rm notes.txt
```

Visual:

```text
Before

notes.txt


After

(deleted)
```

---

# Recursive Delete

```bash
rm -r project
```

Visual:

```text
project

├── src
├── docs
└── tests


↓


Everything Removed
```

---

# File Viewing Ecosystem

```text
File

 │

 ├── cat
 │
 ├── less
 │
 ├── head
 │
 └── tail
```

---

# cat

```text
file.txt

Line 1
Line 2
Line 3
Line 4
```

```bash
cat file.txt
```

Output:

```text
Line 1
Line 2
Line 3
Line 4
```

---

# less

```text
Huge File

10000 Lines
```

Visual:

```text
File
 │
 ▼
less
 │
 ▼
Scrollable View
```

---

# head

```bash
head file.txt
```

Visual:

```text
File

Line 1 ✓
Line 2 ✓
Line 3 ✓
...
```

---

# tail

```bash
tail file.txt
```

Visual:

```text
...
Line 998
Line 999
Line 1000 ✓
```

---

# tail -f

```text
Application

 │
 ▼

app.log

 │
 ▼

tail -f
```

Real-time monitoring:

```text
New Log
     │
     ▼
Screen Updates Automatically
```

---

# Metadata Visualization

```bash
stat file.txt
```

Visual:

```text
file.txt

├── Size
├── Owner
├── Group
├── Permissions
├── Access Time
├── Modify Time
└── Change Time
```

---

# file Command

```bash
file image.png
```

Visual:

```text
Filename
    │
    ▼
Content Analysis
    │
    ▼
PNG Image
```

---

# Search Ecosystem

```text
Searching

├── find
├── locate
├── tree
└── Wildcards
```

---

# find

```bash
find . -name "*.txt"
```

Visual:

```text
Filesystem

/
│
├── docs
│   └── notes.txt ✓
│
├── logs
│   └── app.log
│
└── backup
    └── report.txt ✓
```

---

# find Traversal

```text
Start
 │
 ▼
Directory
 │
 ▼
Subdirectory
 │
 ▼
Subdirectory
 │
 ▼
Match?
 │
 ▼
Result
```

---

# locate

```bash
locate notes.txt
```

Visual:

```text
Filesystem
     │
     ▼

updatedb
     │
     ▼

Database
     │
     ▼

locate
```

---

# find vs locate

```text
find

Filesystem Search
       │
       ▼
Accurate


locate

Database Search
       │
       ▼
Fast
```

---

# tree

```text
project

├── docs
│
├── src
│   ├── api
│   └── ui
│
└── README.md
```

Purpose:

```text
Visualize Hierarchy
```

---

# Wildcard Expansion

User:

```bash
ls *.txt
```

Visual:

```text
User
 │
 ▼

*.txt

 │
 ▼

Shell Expansion

 │
 ▼

notes.txt
report.txt
todo.txt

 │
 ▼

ls Executes
```

---

# Wildcard Types

```text
*

Match Many Characters


?

Match One Character


[]

Character Set


[!]

Negation


{}

Expansion


**

Recursive Match
```

---

# Wildcard Examples

```text
*.txt

 │

 ├── notes.txt
 ├── report.txt
 └── todo.txt
```

---

```text
file?.txt

 │

 ├── file1.txt ✓
 ├── file2.txt ✓
 └── file10.txt ✗
```

---

```text
file[1-3].txt

 │

 ├── file1.txt ✓
 ├── file2.txt ✓
 ├── file3.txt ✓
 └── file4.txt ✗
```

---

# Data Flow Model

Everything after this point becomes:

```text
Data Flow
```

Visual:

```text
Input
 │
 ▼
Process
 │
 ▼
Output
```

---

# Linux Streams

```text
stdin   = 0

stdout  = 1

stderr  = 2
```

Visual:

```text
stdin (0)
     │
     ▼

┌───────────┐
│ Command   │
└───────────┘

     │
     ├────────► stdout (1)
     │
     └────────► stderr (2)
```

---

# Redirection Overview

```text
stdout
   │
   ▼

File
```

---

# Output Redirection

```bash
ls > files.txt
```

Visual:

```text
ls
 │
 ▼

stdout

 │
 ▼

files.txt
```

---

# Append Redirection

```bash
echo "Linux" >> notes.txt
```

Visual:

```text
notes.txt

Line 1

     │
     ▼

Append

     │
     ▼

Line 1
Linux
```

---

# Error Redirection

```bash
ls missing-file 2> error.log
```

Visual:

```text
Command
    │
    ▼

stderr

    │
    ▼

error.log
```

---

# /dev/null

```text
Black Hole
```

Visual:

```text
stdout
    │
    ▼

/dev/null

    │
    ▼

Destroyed
```

---

# Input Redirection

```bash
wc -l < file.txt
```

Visual:

```text
file.txt

   │
   ▼

stdin

   │
   ▼

wc -l
```

---

# Pipe Overview

```bash
ls | wc -l
```

Visual:

```text
Command A
     │
     ▼

Pipe

     │
     ▼

Command B
```

---

# Pipe Internals

```text
stdout
      │
      ▼

Kernel Pipe Buffer

      │
      ▼

stdin
```

---

# Multi-Stage Pipeline

```bash
cat app.log | grep ERROR | sort | uniq
```

Visual:

```text
app.log
   │
   ▼

grep ERROR
   │
   ▼

sort
   │
   ▼

uniq
   │
   ▼

Result
```

---

# Redirection vs Pipe

```text
Redirection

Command
    │
    ▼

File
```

```text
Pipe

Command
    │
    ▼

Command
```

---

# Real DevOps Workflow

```text
Application

     │
     ▼

app.log

     │
     ▼

grep ERROR

     │
     ▼

sort

     │
     ▼

uniq

     │
     ▼

wc -l

     │
     ▼

Error Count
```

---

# Real Security Workflow

```text
Filesystem

     │
     ▼

find

     │
     ▼

Sensitive Files

     │
     ▼

grep

     │
     ▼

Filter

     │
     ▼

Audit Report
```

---

# Complete Learning Map

```text
04-file-operations

├── Navigation
│   ├── pwd
│   ├── ls
│   └── cd
│
├── Creation
│   ├── touch
│   ├── mkdir
│   └── rmdir
│
├── Manipulation
│   ├── cp
│   ├── mv
│   └── rm
│
├── Inspection
│   ├── cat
│   ├── less
│   ├── head
│   ├── tail
│   ├── stat
│   └── file
│
├── Searching
│   ├── find
│   ├── locate
│   ├── tree
│   └── wildcards
│
└── Data Flow
    ├── Redirection
    └── Pipes
```

---

# Final Mental Model

```text
Filesystem
     │
     ▼

Navigate
     │
     ▼

Create
     │
     ▼

Manage
     │
     ▼

Inspect
     │
     ▼

Search
     │
     ▼

Filter
     │
     ▼

Redirect
     │
     ▼

Pipe
     │
     ▼

Automate
```

# One-Sentence Summary

```text
Linux File Operations
=
Managing Files + Understanding Data Flow
```
