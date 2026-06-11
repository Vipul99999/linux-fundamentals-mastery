# Wildcard Expansion (Globbing)

# Introduction

Imagine a folder contains:

```text
notes.txt
report.txt
todo.txt
image.png
video.mp4
```

You want:

```text
All Text Files
```

Without wildcards:

```bash
ls notes.txt report.txt todo.txt
```

Painful.

With wildcards:

```bash
ls *.txt
```

Much easier.

---

# What Is Wildcard Expansion?

Wildcard Expansion (also called **Globbing**) allows the shell to match multiple filenames using patterns.

Example:

```bash
ls *.txt
```

You type:

```text
*.txt
```

But Linux internally expands it to:

```text
notes.txt report.txt todo.txt
```

before executing:

```bash
ls notes.txt report.txt todo.txt
```

---

# Most Important Concept

Many beginners think:

```text
ls understands *.txt
```

Wrong.

Actually:

```text
Shell understands *.txt
```

Visual:

```text
User
 │
 ▼
ls *.txt
 │
 ▼
Shell Expansion
 │
 ▼
ls notes.txt report.txt todo.txt
 │
 ▼
ls Executes
```

---

# Real World Analogy

Imagine a teacher says:

```text
Bring all red books.
```

Students automatically gather:

```text
Red Book 1
Red Book 2
Red Book 3
```

The teacher never listed each book.

Wildcards work the same way.

---

# Why Wildcards Exist

Without wildcards:

```bash
cp file1.txt file2.txt file3.txt backup/
```

With wildcards:

```bash
cp *.txt backup/
```

Much simpler.

---

# Visual Overview

```text
Directory

notes.txt
report.txt
todo.txt
image.png
```

Pattern:

```text
*.txt
```

Matches:

```text
notes.txt
report.txt
todo.txt
```

---

# How Expansion Works

Step 1

User enters:

```bash
ls *.txt
```

Step 2

Shell searches:

```text
Current Directory
```

Step 3

Shell expands:

```text
notes.txt report.txt todo.txt
```

Step 4

Actual command becomes:

```bash
ls notes.txt report.txt todo.txt
```

---

# Wildcard Types

Linux supports several wildcard patterns.

```text
*
?
[]
[^]
{}
```

---

# The Star (*)

Most common wildcard.

Meaning:

```text
Match Zero Or More Characters
```

---

# Visual

Pattern:

```text
*.txt
```

Matches:

```text
notes.txt
report.txt
todo.txt
```

Does NOT match:

```text
image.png
```

---

# Examples

```bash
ls *.txt
```

```bash
rm *.log
```

```bash
cp *.jpg images/
```

---

# Visual Expansion

```text
*.txt

 │

 ├── notes.txt
 ├── report.txt
 └── todo.txt
```

---

# Match Everything

```bash
ls *
```

Visual:

```text
Directory

notes.txt
image.png
video.mp4
README.md
```

Everything matches.

---

# Prefix Matching

Example:

```bash
ls report*
```

Matches:

```text
report
report.txt
report-final.txt
report-2026.pdf
```

---

# Visual

```text
report*
│
├── report
├── report.txt
├── report-final.txt
└── report.pdf
```

---

# Suffix Matching

Example:

```bash
ls *.log
```

Matches:

```text
app.log
nginx.log
system.log
```

---

# Middle Matching

Example:

```bash
ls *backup*
```

Matches:

```text
backup.txt
daily-backup.tar
backup-2026.zip
```

---

# Question Mark (?)

Meaning:

```text
Match Exactly One Character
```

---

# Example

Files:

```text
a.txt
b.txt
ab.txt
```

Command:

```bash
ls ?.txt
```

Matches:

```text
a.txt
b.txt
```

Does NOT match:

```text
ab.txt
```

---

# Visual

```text
?.txt

a.txt   ✓
b.txt   ✓
ab.txt  ✗
```

---

# Multiple Question Marks

Example:

```bash
ls ??.txt
```

Matches:

```text
ab.txt
xy.txt
```

Does NOT match:

```text
a.txt
```

---

# Character Sets []

Match specific characters.

Example:

```bash
ls file[123].txt
```

Matches:

```text
file1.txt
file2.txt
file3.txt
```

---

# Visual

```text
file[123].txt

file1.txt ✓
file2.txt ✓
file3.txt ✓
file4.txt ✗
```

---

# Character Ranges

Example:

```bash
ls file[a-z].txt
```

Matches:

```text
filea.txt
fileb.txt
filez.txt
```

---

# Visual

```text
[a-z]

a
b
c
...
z
```

---

# Numeric Ranges

Example:

```bash
ls file[0-9].txt
```

Matches:

```text
file1.txt
file2.txt
file9.txt
```

---

# Multiple Ranges

Example:

```bash
ls [A-Za-z]*
```

Matches:

```text
Hello.txt
World.txt
Linux.md
```

---

# Negation

Example:

```bash
ls [!0-9]*
```

Meaning:

```text
Anything NOT Starting With Digit
```

---

# Visual

```text
1file.txt   ✗
2file.txt   ✗
notes.txt   ✓
report.txt  ✓
```

---

# Brace Expansion {}

Often confused with wildcards.

Important:

```text
Brace Expansion Happens Before Globbing
```

---

# Example

```bash
echo file{1,2,3}.txt
```

Output:

```text
file1.txt file2.txt file3.txt
```

---

# Visual

```text
{1,2,3}

 │

 ├── 1
 ├── 2
 └── 3
```

---

# Range Expansion

```bash
echo file{1..5}.txt
```

Output:

```text
file1.txt
file2.txt
file3.txt
file4.txt
file5.txt
```

---

# Alphabet Ranges

```bash
echo {a..f}
```

Output:

```text
a b c d e f
```

---

# Visual

```text
a
b
c
d
e
f
```

---

# Combining Expansions

Example:

```bash
touch file{1..5}.txt
```

Creates:

```text
file1.txt
file2.txt
file3.txt
file4.txt
file5.txt
```

---

# Hidden Files

Important Linux behavior.

Files:

```text
.hidden
.config
.bashrc
```

Command:

```bash
ls *
```

Does NOT match hidden files.

---

# Visual

```text
*

notes.txt   ✓
image.png   ✓
.bashrc     ✗
```

---

# Match Hidden Files

```bash
ls .*
```

Matches:

```text
.bashrc
.profile
.config
```

---

# Recursive Globbing

Modern Bash supports:

```bash
shopt -s globstar
```

Then:

```bash
ls **/*.txt
```

Visual:

```text
project
│
├── notes.txt
│
├── docs
│   └── guide.txt
│
└── src
    └── readme.txt
```

All text files found recursively.

---

# Wildcards vs Regular Expressions

Many beginners confuse them.

Wildcard:

```text
*.txt
```

Regex:

```text
.*\.txt$
```

---

# Visual Comparison

```text
Wildcard
│
└── Simple Filename Matching

Regex
│
└── Complex Text Matching
```

---

# Common Commands Using Wildcards

## ls

```bash
ls *.txt
```

## cp

```bash
cp *.jpg images/
```

## mv

```bash
mv *.log archive/
```

## rm

```bash
rm *.tmp
```

## chmod

```bash
chmod 644 *.txt
```

---

# DevOps Examples

Find all YAML files:

```bash
ls *.yaml
```

Find environment files:

```bash
ls .env*
```

Find logs:

```bash
ls *.log
```

---

# Docker Examples

```bash
cp Dockerfile* backups/
```

---

# Kubernetes Examples

```bash
ls *.yaml
```

Matches:

```text
deployment.yaml
service.yaml
ingress.yaml
```

---

# Security Examples

Audit backups:

```bash
ls *.bak
```

Audit logs:

```bash
ls *.log
```

Audit keys:

```bash
ls *.key
```

---

# Dangerous Wildcards

Be careful.

Example:

```bash
rm *
```

Visual:

```text
Current Directory
│
└── EVERYTHING DELETED
```

---

# Extremely Dangerous

```bash
rm -rf *
```

Can destroy entire project contents.

Always verify first:

```bash
ls *
```

---

# Safe Workflow

```text
Preview
  │
  ▼
ls *.log

Verify
  │
  ▼
rm *.log
```

---

# Common Beginner Mistakes

## Expecting Hidden Files

```bash
ls *
```

Does not show:

```text
.bashrc
```

---

## Confusing Regex and Wildcards

Wrong:

```bash
ls .*\.txt$
```

That's regex syntax.

---

## Forgetting Expansion Happens First

Remember:

```text
Shell Expands
Before
Command Executes
```

---

# Visual Memory Map

```text
Wildcard Expansion
│
├── *
│   └── Any Characters
│
├── ?
│   └── One Character
│
├── []
│   └── Character Set
│
├── [!]
│   └── Negation
│
├── {}
│   └── Expansion
│
└── **
    └── Recursive Match
```

---

# Hands-On Lab

Create:

```bash
touch notes.txt
touch report.txt
touch image.png
touch file1.txt
touch file2.txt
touch file3.txt
```

Test:

```bash
ls *.txt
```

Then:

```bash
ls file?.txt
```

Then:

```bash
ls file[1-3].txt
```

Observe differences.

---

# Interview Questions

### What is wildcard expansion?

Shell-based pattern matching used to expand filenames.

---

### Who expands wildcards?

```text
Shell
```

not the command.

---

### What does * mean?

Match zero or more characters.

---

### What does ? mean?

Match exactly one character.

---

### What does [a-z] mean?

Match any lowercase letter.

---

### Difference between wildcard and regex?

Wildcards match filenames.

Regex matches text patterns.

---

# Quick Cheat Sheet

```bash
*

?

[a-z]

[0-9]

[!0-9]

*.txt

file?.txt

file[1-3].txt

{1..10}

{a..z}

**
```

# Key Takeaway

```text
User Types Pattern
        │
        ▼
Shell Expands Pattern
        │
        ▼
Real Filenames Generated
        │
        ▼
Command Executes
```

Think:

```text
Wildcards
=
Filename Matching

Regex
=
Text Matching
```
