# find Command Cheat Sheet

# Quick Syntax

```bash
find [PATH] [CONDITION] [ACTION]
```

Example:

```bash
find . -name "*.txt"
```

Visual:

```text
find
 │
 ▼
Where To Search
 │
 ▼
What To Find
 │
 ▼
What To Do
```

---

# Mental Model

```text
Filesystem
     │
     ▼
Filter
     │
     ▼
Match
     │
     ▼
Action
```

---

# Most Common Commands

## Search By Name

```bash
find . -name notes.txt
```

Find exact filename.

---

## Search By Pattern

```bash
find . -name "*.txt"
```

Find all text files.

---

## Case Insensitive Search

```bash
find . -iname "*.txt"
```

Matches:

```text
notes.txt
NOTES.TXT
Notes.Txt
```

---

## Search Files Only

```bash
find . -type f
```

Visual:

```text
Files ✓

notes.txt
app.js
config.json
```

---

## Search Directories Only

```bash
find . -type d
```

Visual:

```text
Folders ✓

src/
docs/
tests/
```

---

## Search Symbolic Links

```bash
find . -type l
```

---

# Search By Extension

## JavaScript

```bash
find . -name "*.js"
```

## TypeScript

```bash
find . -name "*.ts"
```

## Python

```bash
find . -name "*.py"
```

## Java

```bash
find . -name "*.java"
```

## Go

```bash
find . -name "*.go"
```

## Rust

```bash
find . -name "*.rs"
```

---

# Search By Size

## Larger Than 100MB

```bash
find . -size +100M
```

## Larger Than 1GB

```bash
find . -size +1G
```

## Smaller Than 1MB

```bash
find . -size -1M
```

Visual:

```text
File

2GB   ✓

500KB ✗
```

---

# Search By Time

## Modified Within 24 Hours

```bash
find . -mtime -1
```

## Modified Within 7 Days

```bash
find . -mtime -7
```

## Older Than 30 Days

```bash
find . -mtime +30
```

Visual:

```text
Today
 │
 ▼
7 Days Ago
```

---

# Access Time

```bash
find . -atime -7
```

Recently accessed files.

---

# Change Time

```bash
find . -ctime -7
```

Recently changed metadata.

---

# Search By Owner

```bash
find . -user root
```

---

# Search By Group

```bash
find . -group developers
```

---

# Search By Permissions

## Permission 644

```bash
find . -perm 644
```

## Permission 755

```bash
find . -perm 755
```

---

# Security Searches

## World Writable Files

```bash
find / -perm -002
```

---

## World Writable Directories

```bash
find / -type d -perm -002
```

---

## SUID Files

```bash
find / -perm -4000
```

---

## SGID Files

```bash
find / -perm -2000
```

---

## Executable Files

```bash
find . -type f -executable
```

---

# Empty Files & Directories

## Empty Files

```bash
find . -type f -empty
```

---

## Empty Directories

```bash
find . -type d -empty
```

---

# Search Logic

## AND

Default behavior:

```bash
find . -name "*.js" -size +1M
```

Meaning:

```text
JavaScript
AND
Size > 1MB
```

---

## OR

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

## NOT

```bash
find . ! -name "*.log"
```

Meaning:

```text
Everything
Except Logs
```

---

# Depth Control

## Current Directory Only

```bash
find . -maxdepth 1
```

---

## Skip Top Levels

```bash
find . -mindepth 2
```

---

# Excluding Directories

## Skip node_modules

```bash
find . -path "./node_modules" -prune -o -name "*.js"
```

Visual:

```text
project
│
├── src ✓
│
└── node_modules ✗
```

---

# Actions

## Execute Command

```bash
find . -name "*.log" -exec ls -lh {} \;
```

Visual:

```text
Find
 │
 ▼
Run Command
```

---

## Delete Results

```bash
find . -name "*.tmp" -delete
```

---

## Print Results

```bash
find . -name "*.log" -print
```

---

# xargs

```bash
find . -name "*.log" | xargs rm
```

Useful for bulk operations.

---

# DevOps Commands

## Dockerfiles

```bash
find . -name Dockerfile
```

---

## Docker Compose

```bash
find . -name "docker-compose*.yml"
```

---

## Kubernetes YAML

```bash
find . -name "*.yaml"
```

---

## Helm Charts

```bash
find . -name Chart.yaml
```

---

## Environment Files

```bash
find . -name ".env*"
```

---

# Programming Projects

## Node.js

```bash
find . -name package.json
```

---

## Python

```bash
find . -name requirements.txt
```

---

## Go

```bash
find . -name go.mod
```

---

## Java Maven

```bash
find . -name pom.xml
```

---

## Gradle

```bash
find . -name build.gradle
```

---

## Rust

```bash
find . -name Cargo.toml
```

---

# AI / Data Science

## Datasets

```bash
find datasets -name "*.csv"
```

---

## Model Files

```bash
find models -name "*.pt"
find models -name "*.onnx"
find models -name "*.safetensors"
```

---

## Experiment Logs

```bash
find logs -name "*.log"
```

---

# Incident Response

## Recently Modified Files

```bash
find /etc -mtime -1
```

---

## Recently Accessed Files

```bash
find /home -atime -1
```

---

## Large Files

```bash
find / -size +1G
```

---

## Core Dumps

```bash
find / -name "core*"
```

---

# Certificate Audits

## Certificates

```bash
find /etc -name "*.crt"
```

---

## Private Keys

```bash
find /etc -name "*.key"
```

---

# Log Investigation

## All Logs

```bash
find /var/log -type f
```

---

## Recent Logs

```bash
find /var/log -mtime -1
```

---

# Backup Discovery

## TAR Archives

```bash
find / -name "*.tar"
```

---

## GZIP Archives

```bash
find / -name "*.tar.gz"
```

---

## ZIP Archives

```bash
find / -name "*.zip"
```

---

# Performance Tips

## Bad

```bash
find /
```

Visual:

```text
Entire System
│
└── Millions Of Files
```

---

## Better

```bash
find /var/log
```

Visual:

```text
Specific Directory
│
└── Smaller Search Space
```

---

# find vs locate

```text
find
│
├── Real-Time
├── Accurate
└── Slower

locate
│
├── Indexed Database
├── Fast
└── May Be Outdated
```

---

# Common Mistakes

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

## Deleting Without Preview

Risky:

```bash
find . -name "*.tmp" -delete
```

Safer:

```bash
find . -name "*.tmp"
```

Review results first.

---

# Visual Memory Map

```text
find
│
├── Name
│   └── -name
│
├── Type
│   └── -type
│
├── Size
│   └── -size
│
├── Time
│   └── -mtime
│
├── Owner
│   └── -user
│
├── Group
│   └── -group
│
├── Permissions
│   └── -perm
│
├── Execute
│   └── -exec
│
└── Delete
    └── -delete
```

---

# 30-Second Interview Revision

```text
Search By Name
  -name

Search By Type
  -type f
  -type d

Search By Size
  -size +100M

Search By Time
  -mtime -7

Search By Owner
  -user

Search By Permission
  -perm

Execute Commands
  -exec

Delete Results
  -delete

Exclude Paths
  -prune

Limit Depth
  -maxdepth
  -mindepth
```

---

# Golden Rule

```text
Search First
Review Results
Then Execute Actions

find → Verify → Delete

Never:

find → Delete Immediately
```
