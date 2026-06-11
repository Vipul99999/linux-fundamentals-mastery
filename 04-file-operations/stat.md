# stat Command

# What is stat?

`stat` stands for:

```text
Status
```

The `stat` command displays detailed information (metadata) about a file or directory.

Think of it as:

```text
File Identity Card
```

or

```text
File Passport
```

While commands like:

```text
cat
```

show:

```text
What's inside a file
```

`stat` shows:

```text
Everything Linux knows about that file
```

---

# Why Do We Need stat?

Imagine a school notebook.

Looking at the cover:

```text
Math Notebook
```

only tells you its name.

But a school record might tell you:

```text
Owner
Creation Date
Size
Last Modified
Class
ID Number
```

Linux files also have records.

`stat` displays those records.

---

# Real-Life Analogy

Imagine a passport.

Passport contains:

```text
Name
Date of Birth
Nationality
Passport Number
Issue Date
Expiry Date
```

A Linux file also has metadata.

```text
File Name
Owner
Permissions
Size
Timestamps
Inode Number
```

`stat` displays the file's passport.

---

# Visual Overview

```text
notes.txt

┌───────────────────────┐
│ CONTENT               │
│ Hello Linux           │
└───────────────────────┘

          +

┌───────────────────────┐
│ METADATA              │
│ Owner                 │
│ Size                  │
│ Permissions           │
│ Inode                 │
│ Access Time           │
│ Modify Time           │
└───────────────────────┘
```

---

# Why Metadata Matters

Without metadata Linux cannot know:

```text
Who owns file?
Who can read file?
How large is file?
Where is file stored?
When was it changed?
```

Metadata is critical.

---

# Syntax

```bash
stat filename
```

Example:

```bash
stat notes.txt
```

---

# Your First Example

Create:

```bash
touch notes.txt
```

Run:

```bash
stat notes.txt
```

Example output:

```text
File: notes.txt
Size: 0
Blocks: 0
IO Block: 4096
regular empty file
Device: 8,1
Inode: 123456
Links: 1
Access: (0644/-rw-r--r--)
Uid: (1000/user)
Gid: (1000/user)
Access: 2026-01-01
Modify: 2026-01-01
Change: 2026-01-01
 Birth: 2026-01-01
```

Looks scary.

Let's decode it.

---

# Visual Breakdown

```text
notes.txt
│
├── Size
├── Inode
├── Permissions
├── Owner
├── Group
├── Access Time
├── Modify Time
└── Change Time
```

---

# File Name

Example:

```text
File: notes.txt
```

This is the file you requested.

---

# File Size

Example:

```text
Size: 1024
```

Meaning:

```text
1024 Bytes
```

Visual:

```text
Small File

┌───────┐
│ Data  │
└───────┘

1024 Bytes
```

---

# Size Examples

```text
1 KB  = 1024 Bytes

1 MB  = 1024 KB

1 GB  = 1024 MB
```

---

# Visual Size Comparison

```text
Text File
│
└── Few KB

Image
│
└── Few MB

Movie
│
└── Several GB
```

---

# What Is an Inode?

One of the most important Linux concepts.

Every file receives:

```text
Inode Number
```

Example:

```text
Inode: 123456
```

Think of it as:

```text
File ID Card Number
```

---

# Visual Inode Concept

```text
Filename
│
├── notes.txt
│
▼

Inode
│
└── 123456
```

Linux internally tracks files using inode numbers.

Not filenames.

---

# Real-Life Analogy

People:

```text
Name:
John

ID:
928374
```

Many people can have same name.

ID is unique.

Linux files work similarly.

---

# Visual

```text
notes.txt
      │
      ▼
   Inode
      │
      ▼
   123456
```

---

# What Are Permissions?

Example:

```text
-rw-r--r--
```

Visual:

```text
Owner  Group  Others
 │       │       │
rw      r       r
```

Meaning:

```text
Owner:
Read Write

Group:
Read

Others:
Read
```

---

# Permission Visualization

```text
File
│
├── Owner Can Read
├── Owner Can Write
├── Group Can Read
└── Others Can Read
```

---

# Owner Information

Example:

```text
Uid: user
```

Meaning:

```text
This file belongs to user
```

---

Visual:

```text
notes.txt
│
└── Owner
     │
     └── user
```

---

# Group Information

Example:

```text
Gid: developers
```

Visual:

```text
notes.txt
│
└── Group
     │
     └── developers
```

Groups help share access.

---

# Timestamps

One of the most important sections.

Linux tracks multiple times.

```text
Access Time (atime)

Modify Time (mtime)

Change Time (ctime)
```

---

# Visual Overview

```text
File
│
├── Access Time
├── Modify Time
└── Change Time
```

---

# Access Time (atime)

Records:

```text
When file was last read
```

Example:

```bash
cat notes.txt
```

updates access time.

---

Visual:

```text
Read File
     │
     ▼
Update atime
```

---

# Modify Time (mtime)

Records:

```text
When content changed
```

Example:

```bash
echo "Hello" >> notes.txt
```

updates:

```text
mtime
```

---

Visual

```text
Content Changed
      │
      ▼
Update mtime
```

---

# Change Time (ctime)

Records:

```text
Metadata Changes
```

Examples:

```bash
chmod
chown
```

update ctime.

---

Visual

```text
Permission Changed
        │
        ▼
Update ctime
```

---

# Timestamp Comparison

| Time  | Meaning          |
| ----- | ---------------- |
| atime | Last Read        |
| mtime | Content Modified |
| ctime | Metadata Changed |

---

# Directory Metadata

Directories also have metadata.

Example:

```bash
mkdir project

stat project
```

Output includes:

```text
Owner
Permissions
Size
Timestamps
```

---

# Visual

```text
project
│
├── Metadata
│
├── Permissions
├── Owner
└── Times
```

---

# What Happens Internally?

When you run:

```bash
stat notes.txt
```

Linux performs:

```text
Filename
     │
     ▼
Find Inode
     │
     ▼
Read Metadata
     │
     ▼
Display Information
```

---

# Internal Visualization

```text
notes.txt
     │
     ▼
Filesystem
     │
     ▼
Inode
     │
     ▼
Metadata
     │
     ▼
stat Output
```

---

# Useful Options

## Show Filesystem Information

```bash
stat -f notes.txt
```

Displays filesystem details.

---

## Custom Format

```bash
stat -c "%n %s"
```

Output:

```text
notes.txt 1024
```

---

# Common Format Specifiers

| Specifier | Meaning |
| --------- | ------- |
| %n        | Name    |
| %s        | Size    |
| %i        | Inode   |
| %U        | Owner   |
| %G        | Group   |

---

# Example

```bash
stat -c "%n %s %U" notes.txt
```

Output:

```text
notes.txt 1024 user
```

---

# Real-World Use Cases

## Verify File Size

```bash
stat backup.zip
```

---

## Check Ownership

```bash
stat config.yaml
```

---

## Investigate Changes

```bash
stat app.log
```

Check timestamps.

---

## Security Audits

```bash
stat ssh_config
```

Verify permissions.

---

## Forensics

```bash
stat suspicious_file
```

Check:

```text
Owner
Time
Permissions
```

---

# Hands-On Lab

Create:

```bash
touch notes.txt
```

Check:

```bash
stat notes.txt
```

Observe:

```text
Size
Permissions
Inode
Times
```

---

Add content:

```bash
echo "Hello Linux" > notes.txt
```

Check again:

```bash
stat notes.txt
```

Notice:

```text
Size Increased
mtime Changed
```

---

Read file:

```bash
cat notes.txt
```

Check:

```bash
stat notes.txt
```

Observe:

```text
atime Updated
```

---

Change permissions:

```bash
chmod 777 notes.txt
```

Check:

```bash
stat notes.txt
```

Observe:

```text
ctime Updated
```

---

# Common Beginner Mistakes

## Confusing Size With Disk Usage

```text
File Size
```

and

```text
Disk Usage
```

are not always identical.

---

## Ignoring Timestamps

Timestamps are extremely useful for troubleshooting.

---

## Thinking Filename Is File Identity

Linux primarily tracks files using:

```text
Inode Numbers
```

not filenames.

---

# Memory Trick

Think:

```text
cat
│
└── Read Notebook

stat
│
└── Read Notebook Passport
```

---

# Visual Memory Map

```text
File
│
├── Content
│     │
│     └── cat
│
└── Metadata
      │
      └── stat
```

---

# Interview Questions

## What does stat do?

Displays detailed metadata about files and directories.

---

## What is an inode?

A unique filesystem identifier used internally by Linux to track files.

---

## Difference Between atime and mtime?

```text
atime
│
└── Last Read

mtime
│
└── Last Content Change
```

---

## What is ctime?

Records metadata changes.

---

## Can stat work on directories?

Yes.

---

## Why is stat useful?

For:

```text
Troubleshooting
Security
Forensics
Permissions
File Analysis
```

---

# Quick Summary

```text
Command:
stat

Purpose:
View File Metadata

Shows:

File Name
Size
Permissions
Owner
Group
Inode
Access Time
Modify Time
Change Time

Remember:

cat
│
└── Content

stat
│
└── Metadata
```
