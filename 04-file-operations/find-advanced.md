# find Advanced Guide

# Introduction

Basic `find` usage is easy:

```bash
find . -name "*.txt"
```

But real-world engineers often need:

```text
Find files larger than 1GB
AND

Modified in last 7 days
AND

Owned by nginx
AND

Not inside backups
```

This is where advanced `find` becomes powerful.

---

# Visual Mental Model

Think of `find` as a filter pipeline.

```text
Filesystem
     │
     ▼
File
     │
     ▼
Condition 1
     │
     ▼
Condition 2
     │
     ▼
Condition 3
     │
     ▼
Output
```

Only files that pass every condition survive.

---

# Understanding Expressions

Basic:

```bash
find . -name "*.log"
```

Advanced:

```bash
find . -name "*.log" -size +100M
```

Meaning:

```text
Must be:

Log File
AND
Larger Than 100 MB
```

---

# Visual

```text
app.log      200MB ✓

small.log     5MB ✗

notes.txt   200MB ✗
```

---

# AND Logic

Default behavior is AND.

```bash
find . -name "*.js" -size +1M
```

Visual:

```text
JavaScript?
      │
      ▼
Yes
      │
      ▼
Larger Than 1MB?
      │
 ┌────┴────┐
 │         │
Yes       No
 │         │
 ▼         ▼
Keep     Ignore
```

---

# OR Logic

Use:

```bash
find . \( -name "*.js" -o -name "*.ts" \)
```

Meaning:

```text
JavaScript
OR
TypeScript
```

---

# Visual

```text
app.js    ✓

app.ts    ✓

app.py    ✗
```

---

# NOT Logic

Exclude files.

```bash
find . ! -name "*.log"
```

Visual:

```text
app.log      ✗

notes.txt    ✓

image.png    ✓
```

---

# Grouping Conditions

Example:

```bash
find . \( -name "*.js" -o -name "*.ts" \) -size +1M
```

Visual:

```text
(JS OR TS)
      │
      ▼
Size > 1MB
      │
      ▼
Result
```

---

# Search By Multiple Extensions

```bash
find . \( -name "*.jpg" -o -name "*.png" -o -name "*.webp" \)
```

Visual:

```text
photo.jpg    ✓

image.png    ✓

logo.webp    ✓

video.mp4    ✗
```

---

# Advanced Time Searches

Modified within 24 hours:

```bash
find . -mtime -1
```

Modified within 30 days:

```bash
find . -mtime -30
```

Older than 1 year:

```bash
find . -mtime +365
```

---

# Visual Timeline

```text
Today
 │
 ▼
1 Day Ago
 │
 ▼
7 Days Ago
 │
 ▼
30 Days Ago
 │
 ▼
365 Days Ago
```

---

# Access Time Search

Find files recently read:

```bash
find . -atime -7
```

Visual:

```text
Opened Recently
      │
      ▼
Found
```

---

# Change Time Search

Find files whose metadata changed:

```bash
find . -ctime -7
```

Useful for:

```text
Permission Changes
Ownership Changes
Security Audits
```

---

# Search By Permissions

Find world writable files:

```bash
find / -perm -002
```

Visual:

```text
secure.txt

rw-r--r--
      ✗

public.txt

rw-rw-rw-
      ✓
```

---

# Search By Exact Permissions

```bash
find . -perm 644
```

Visual:

```text
644 ✓

600 ✗

755 ✗
```

---

# Search By Owner

```bash
find . -user nginx
```

Visual:

```text
Owner

nginx ✓

root ✗
```

---

# Search By Group

```bash
find . -group developers
```

---

# Search Empty Directories

```bash
find . -type d -empty
```

Visual:

```text
docs/
│
└── empty ✓

src/
│
└── files ✗
```

---

# Search Empty Files

```bash
find . -type f -empty
```

Visual:

```text
notes.txt
│
└── 0 bytes ✓
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

# Broken Symlink Detection

```bash
find . -xtype l
```

Visual:

```text
shortcut
   │
   ▼
missing-file

Broken ✓
```

---

# Depth Control

Limit search depth.

Search current directory only:

```bash
find . -maxdepth 1
```

Visual:

```text
Current Folder ✓

Subfolders ✗
```

---

# Search Deep Levels Only

```bash
find . -mindepth 2
```

Visual:

```text
Level 0 ✗

Level 1 ✗

Level 2 ✓

Level 3 ✓
```

---

# Pruning Directories

Skip node_modules:

```bash
find . -path "./node_modules" -prune -o -name "*.js"
```

Visual:

```text
Project
│
├── src        ✓
│
├── tests      ✓
│
└── node_modules
      ✗ skipped
```

---

# Why Pruning Matters

Without prune:

```text
Project
│
└── node_modules
    │
    └── 100,000 files
```

Search becomes slow.

---

# Search and Execute

One of find's most powerful features.

```bash
find . -name "*.log" -exec ls -lh {} \;
```

Workflow:

```text
Find File
     │
     ▼
Run Command
     │
     ▼
Show Output
```

---

# Understanding {}

```text
{}
```

means:

```text
Current Found File
```

Example:

```bash
find . -name "*.txt" -exec cat {} \;
```

Equivalent to:

```text
cat file1.txt

cat file2.txt

cat file3.txt
```

---

# Bulk Rename Example

```bash
find . -name "*.TXT" -exec mv {} {}.bak \;
```

Illustrates how actions can be automated.

---

# Using xargs

Instead of:

```bash
find . -name "*.log" -exec rm {} \;
```

Use:

```bash
find . -name "*.log" | xargs rm
```

Better performance for large numbers of files.

---

# Visual

```text
Find Results
      │
      ▼
xargs
      │
      ▼
Single Batch Command
```

---

# Delete Files Safely

Preview first:

```bash
find . -name "*.tmp"
```

Then:

```bash
find . -name "*.tmp" -delete
```

Never delete before verifying.

---

# Production Troubleshooting

# Find Huge Files

```bash
find / -size +1G
```

Visual:

```text
backup.tar     10GB ✓

video.mp4       5GB ✓

notes.txt       2KB ✗
```

Useful when:

```text
Disk Full
```

---

# Find Recent Changes

```bash
find /etc -mtime -1
```

Useful for:

```text
Configuration Audits
Incident Response
```

---

# Find Core Dumps

```bash
find / -name "core*"
```

Useful for debugging crashes.

---

# Security Auditing

Find SUID binaries:

```bash
find / -perm -4000
```

Visual:

```text
sudo ✓

passwd ✓

su ✓
```

---

# DevOps Examples

Find Dockerfiles:

```bash
find . -name Dockerfile
```

Find Kubernetes manifests:

```bash
find . -name "*.yaml"
```

Find Helm charts:

```bash
find . -name Chart.yaml
```

---

# AI / Data Science Examples

Find datasets:

```bash
find datasets -name "*.csv"
```

Find model files:

```bash
find models -name "*.pt"
find models -name "*.onnx"
find models -name "*.safetensors"
```

Find experiment logs:

```bash
find logs -name "*.log"
```

---

# Performance Optimization

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
Entire System
│
└── 10 Million Files

Specific Folder
│
└── 10 Thousand Files
```

Search the smallest possible subtree.

---

# Common Beginner Mistakes

## Missing Quotes

Wrong:

```bash
find . -name *.txt
```

Correct:

```bash
find . -name "*.txt"
```

---

## Using -delete Without Preview

Dangerous:

```bash
find . -name "*.tmp" -delete
```

Safer:

```bash
find . -name "*.tmp"
```

Review results first.

---

## Searching Entire Filesystem Unnecessarily

Avoid:

```bash
find /
```

when a smaller directory is known.

---

# Visual Cheat Sheet

```text
find
│
├── Logic
│   ├── AND
│   ├── OR
│   └── NOT
│
├── Filters
│   ├── Name
│   ├── Type
│   ├── Size
│   ├── Time
│   ├── Owner
│   └── Permissions
│
├── Actions
│   ├── -exec
│   ├── xargs
│   └── -delete
│
└── Optimization
    ├── -maxdepth
    ├── -mindepth
    └── -prune
```

# Memory Trick

```text
Basic find
│
└── Search

Advanced find
│
├── Search
├── Filter
├── Automate
├── Audit
└── Investigate
```

# Interview Questions

### What is the default logical operator in find?

```text
AND
```

### How do you perform OR logic?

```bash
find . \( condition1 -o condition2 \)
```

### How do you exclude files?

```bash
find . ! condition
```

### How do you skip directories?

```bash
-prune
```

### How do you execute commands on results?

```bash
-exec command {} \;
```

### Why is xargs often faster than -exec?

Because it batches multiple files into fewer command executions.

# Quick Summary

```text
Advanced find = Search + Logic + Automation

Most Important Features:

AND
OR
NOT
-maxdepth
-mindepth
-prune
-exec
-delete
xargs
Permissions
Ownership
Time Filters
```
