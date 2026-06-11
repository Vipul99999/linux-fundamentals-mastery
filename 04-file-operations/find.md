# find Command - The Ultimate Guide

# What is find?

The `find` command is Linux's search engine for files and directories.

Imagine a city containing millions of houses.

```text
City
│
├── Area A
├── Area B
├── Area C
└── Area D
```

You need to locate:

```text
notes.txt
```

Would you manually open every building?

Of course not.

Linux uses:

```bash
find
```

to search automatically.

---

# Why find Exists

Small projects:

```text
project
│
├── app.js
├── config.json
└── README.md
```

Easy to navigate.

Large projects:

```text
monorepo
│
├── service-a
├── service-b
├── service-c
├── service-d
├── service-e
│
└── 500,000+ files
```

Manual searching becomes impossible.

---

# Real World Analogy

Think of a librarian.

```text
Library
│
├── Science
├── Math
├── History
└── Programming
```

You ask:

```text
Find "Linux Fundamentals"
```

The librarian searches every shelf.

Linux does exactly this.

---

# Visual Overview

```text
Filesystem

/
│
├── home
│   │
│   ├── vip
│   │   │
│   │   ├── notes.txt
│   │   └── project
│   │
│   └── admin
│
├── etc
│
└── var
```

Command:

```bash
find / -name notes.txt
```

Search path:

```text
/
│
├── home
│
├── etc
│
└── var
```

Result:

```text
/home/vip/notes.txt
```

---

# How find Works Internally

When you run:

```bash
find . -name "*.txt"
```

Linux performs:

```text
Start Directory
      │
      ▼
Read Entries
      │
      ▼
Check Conditions
      │
      ▼
Match?
      │
 ┌────┴────┐
 │         │
Yes       No
 │         │
 ▼         ▼
Print   Continue
```

---

# Understanding Recursion

find searches recursively.

Meaning:

```text
Current Folder
    │
    ▼
Subfolder
    │
    ▼
Subfolder
    │
    ▼
Subfolder
```

until every location is checked.

---

# Syntax

```bash
find [path] [expression]
```

Example:

```bash
find . -name "*.txt"
```

Breakdown:

```text
find
 │
 ▼
Search Command

.
 │
 ▼
Current Directory

-name "*.txt"
 │
 ▼
Search Condition
```

---

# Understanding Search Paths

Current directory:

```bash
find .
```

Home directory:

```bash
find ~
```

Specific directory:

```bash
find /var/log
```

Entire system:

```bash
find /
```

---

# Search By Name

Most common usage.

```bash
find . -name notes.txt
```

Visual:

```text
Directory

notes.txt    ✓
report.txt   ✗
image.jpg    ✗
```

Result:

```text
./notes.txt
```

---

# Wildcard Searching

Find all text files:

```bash
find . -name "*.txt"
```

Visual:

```text
notes.txt     ✓
report.txt    ✓
image.png     ✗
video.mp4     ✗
```

---

# Case Sensitive vs Case Insensitive

Case Sensitive:

```bash
find . -name README.md
```

Matches:

```text
README.md
```

Does NOT match:

```text
readme.md
```

Case Insensitive:

```bash
find . -iname readme.md
```

Matches:

```text
README.md
readme.md
ReadMe.md
README.MD
```

---

# Search Files Only

```bash
find . -type f
```

Visual:

```text
app.js
config.json
notes.txt
```

---

# Search Directories Only

```bash
find . -type d
```

Visual:

```text
src
docs
tests
```

---

# Search Symbolic Links

```bash
find . -type l
```

Visual:

```text
shortcut
 │
 ▼
actual-file
```

---

# Search Empty Files

```bash
find . -empty
```

Visual:

```text
notes.txt
│
└── Empty ✓
```

---

# Search By Size

Files larger than 100MB:

```bash
find . -size +100M
```

Visual:

```text
movie.mp4     2GB ✓
photo.jpg     5MB ✗
notes.txt     2KB ✗
```

---

# Size Units

```text
c = Bytes
k = KB
M = MB
G = GB
```

Examples:

```bash
find . -size +1G
find . -size +500M
find . -size -10k
```

---

# Search By Time

Modified within last 7 days:

```bash
find . -mtime -7
```

Visual Timeline:

```text
Today
 │
 ▼
7 Days Ago
 │
 ▼
Modified File ✓
```

---

# Search Old Files

Older than 30 days:

```bash
find . -mtime +30
```

Useful for:

```text
Cleanup
Archiving
Auditing
```

---

# Search By Owner

```bash
find . -user vip
```

Visual:

```text
notes.txt
│
└── Owner: vip
```

---

# Search By Group

```bash
find . -group developers
```

---

# Search By Permissions

World writable files:

```bash
find / -perm -002
```

Visual:

```text
secure.txt     rw-r--r--
public.txt     rw-rw-rw- ✓
```

---

# Combining Conditions

Find JavaScript files larger than 1MB:

```bash
find . -name "*.js" -size +1M
```

Visual:

```text
app.js      2MB ✓
small.js   10KB ✗
```

---

# AND Logic

Default behavior:

```bash
find . -name "*.js" -size +1M
```

Meaning:

```text
Must Be JavaScript
AND
Must Be Larger Than 1MB
```

---

# OR Logic

```bash
find . \( -name "*.js" -o -name "*.ts" \)
```

Visual:

```text
app.js    ✓
app.ts    ✓
app.py    ✗
```

---

# NOT Logic

```bash
find . ! -name "*.log"
```

Meaning:

```text
Everything
Except
Log Files
```

---

# Execute Commands

One of find's most powerful features.

Find and delete empty files:

```bash
find . -empty -delete
```

Visual:

```text
Find Empty Files
       │
       ▼
Delete
```

---

# Using -exec

Display file information:

```bash
find . -name "*.log" -exec ls -lh {} \;
```

Workflow:

```text
Find Log File
      │
      ▼
Run ls -lh
      │
      ▼
Display Details
```

---

# Modern Development Use Cases

## Find Dockerfiles

```bash
find . -name Dockerfile
```

Visual:

```text
service-a
│
└── Dockerfile

service-b
│
└── Dockerfile
```

---

## Find Kubernetes Files

```bash
find . -name "*.yaml"
```

---

## Find Environment Files

```bash
find . -name ".env*"
```

---

## Find Source Code

```bash
find . -name "*.js"
find . -name "*.ts"
find . -name "*.go"
```

---

# Security Use Cases

Find SUID binaries:

```bash
find / -perm -4000
```

Find executable files:

```bash
find . -type f -executable
```

Find recently modified files:

```bash
find /etc -mtime -1
```

Used in:

```text
Incident Response
Forensics
Security Audits
```

---

# AI & Data Science Use Cases

Find datasets:

```bash
find datasets -name "*.csv"
```

Find models:

```bash
find models -name "*.pt"
find models -name "*.onnx"
find models -name "*.safetensors"
```

Find training logs:

```bash
find logs -name "*.log"
```

---

# Performance Tips

Bad:

```bash
find /
```

Good:

```bash
find /var/log
```

Visual:

```text
Entire Filesystem
│
└── 10,000,000 Files

Specific Directory
│
└── 10,000 Files
```

Search the smallest possible subtree.

---

# find vs locate

```text
find
│
├── Real Time
├── Accurate
└── Slower

locate
│
├── Indexed Database
├── Extremely Fast
└── May Be Outdated
```

---

# Common Beginner Mistakes

## Forgetting Quotes

Wrong:

```bash
find . -name *.txt
```

Correct:

```bash
find . -name "*.txt"
```

---

## Searching From Root Accidentally

```bash
find /
```

May take a very long time.

---

## Confusing Files and Directories

```text
-type f = Files

-type d = Directories
```

---

# Visual Cheat Sheet

```text
find
│
├── Search By Name
│   └── -name
│
├── Search By Type
│   └── -type
│
├── Search By Size
│   └── -size
│
├── Search By Time
│   └── -mtime
│
├── Search By Owner
│   └── -user
│
├── Search By Group
│   └── -group
│
├── Search By Permission
│   └── -perm
│
├── Execute Commands
│   └── -exec
│
└── Delete Results
    └── -delete
```

# Memory Trick

```text
ls
│
└── Show What Is Here

find
│
└── Search Everywhere
```

# Interview Questions

### What is the purpose of find?

Search files and directories recursively.

### Difference between find and locate?

```text
find
│
└── Real-time search

locate
│
└── Database search
```

### How do you find all .txt files?

```bash
find . -name "*.txt"
```

### How do you find directories only?

```bash
find . -type d
```

### How do you find files larger than 1GB?

```bash
find . -size +1G
```

### How do you execute a command on found files?

```bash
find . -exec command {} \;
```

# Quick Summary

```text
find = Linux Search Engine

Most Important Options:

-name
-iname
-type
-size
-mtime
-user
-group
-perm
-exec
-delete
```
