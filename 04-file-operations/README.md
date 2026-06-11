# File Operations in Linux

> "Everything in Linux is a file."
>
> Mastering file operations is one of the most important skills in Linux. Every task—from software development and system administration to cybersecurity and cloud engineering—depends on understanding how files and directories are created, viewed, modified, copied, moved, searched, and deleted.

---

# Learning Objectives

After completing this module, you will be able to:

✅ Navigate the filesystem confidently

✅ Create and manage files and directories

✅ Copy, move, rename, and delete files safely

✅ View file contents efficiently

✅ Search files across the filesystem

✅ Understand file metadata

✅ Use redirection and pipes like a Linux professional

✅ Automate file operations using shell patterns

✅ Solve real-world Linux administration tasks

✅ Answer Linux interview questions confidently

---

# Why File Operations Matter

Every Linux activity involves files.

Examples:

| Activity | Uses Files? |
|-----------|------------|
| Writing code | ✅ |
| Running programs | ✅ |
| Installing software | ✅ |
| Managing servers | ✅ |
| Logs and monitoring | ✅ |
| Databases | ✅ |
| Networking configs | ✅ |
| User management | ✅ |

Without file operations, Linux administration is impossible.

---

# Module Roadmap

```text
04-file-operations/
│
├── README.md
│
├── Navigation
│   ├── pwd.md
│   ├── ls.md
│   └── cd.md
│
├── Directory Management
│   ├── mkdir.md
│   └── rmdir.md
│
├── File Creation
│   └── touch.md
│
├── File Manipulation
│   ├── cp.md
│   ├── mv.md
│   └── rm.md
│
├── File Viewing
│   ├── cat.md
│   ├── less.md
│   └── head-tail.md
│
├── File Information
│   ├── stat.md
│   └── file-command.md
│
├── Searching
│   ├── find.md
│   ├── locate.md
│   └── tree.md
│
├── Shell Features
│   ├── wildcard-expansion.md
│   ├── redirection.md
│   └── pipes.md
│
├── interview-questions.md
└── references.md
```

---

# Big Picture

```text
                FILE OPERATIONS
                       │
     ┌─────────────────┼─────────────────┐
     │                 │                 │
 Navigation      File Management    File Viewing
     │                 │                 │
 pwd              touch             cat
 ls               cp                less
 cd               mv                head
                  rm                tail
     │
     │
     ▼
 Search & Metadata
     │
 find
 locate
 stat
 file

     │
     ▼
 Shell Power
     │
 Wildcards
 Redirection
 Pipes
```

---

# Linux File Lifecycle

Understanding what happens to a file throughout its life:

```text
Create
  │
  ▼
Modify
  │
  ▼
Copy
  │
  ▼
Move/Rename
  │
  ▼
View/Search
  │
  ▼
Delete
```

Linux commands map directly to this lifecycle.

```text
Create     → touch
Modify     → editors
Copy       → cp
Move       → mv
View       → cat/less
Search     → find
Delete     → rm
```

---

# Files vs Directories

A beginner mistake is confusing files and directories.

## File

Stores data.

Examples:

```text
notes.txt
app.js
report.pdf
config.yaml
```

---

## Directory

Stores files and other directories.

Examples:

```text
Documents/
Projects/
Downloads/
Music/
```

---

Visual:

```text
home
│
├── notes.txt
├── report.pdf
│
└── Projects
    │
    ├── app.js
    └── backend
```

---

# Essential Navigation Commands

## pwd

Shows current location.

```bash
pwd
```

Output:

```text
/home/vip/projects
```

---

## ls

Lists contents.

```bash
ls
```

Output:

```text
file1.txt
file2.txt
Documents
```

---

## cd

Changes directory.

```bash
cd Documents
```

---

Navigation Flow:

```text
/
│
├── home
│   └── vip
│       └── Projects
│
└── etc
```

```bash
cd /home/vip/Projects
```

Moves directly to Projects.

---

# Directory Management

## Create Directory

```bash
mkdir project
```

Result:

```text
project/
```

---

## Create Nested Directories

```bash
mkdir -p app/src/components
```

Result:

```text
app
└── src
    └── components
```

---

## Remove Empty Directory

```bash
rmdir project
```

---

# File Creation

## touch

Create empty files.

```bash
touch notes.txt
```

Result:

```text
notes.txt
```

---

Create multiple files:

```bash
touch a.txt b.txt c.txt
```

---

# Copy Files

## cp

```bash
cp source.txt backup.txt
```

Visual:

```text
Before

source.txt

After

source.txt
backup.txt
```

---

Copy directory:

```bash
cp -r project backup
```

---

# Move and Rename

## mv

Move file:

```bash
mv notes.txt Documents/
```

Rename file:

```bash
mv old.txt new.txt
```

Visual:

```text
old.txt
   │
   ▼
new.txt
```

---

# Remove Files

## rm

Delete file:

```bash
rm notes.txt
```

Delete directory recursively:

```bash
rm -r project
```

Delete forcefully:

```bash
rm -rf project
```

⚠ Warning:

```text
rm -rf /
```

Can destroy a Linux system if protections are bypassed.

Always verify before using recursive deletion.

---

# Viewing File Content

## cat

Display entire file.

```bash
cat notes.txt
```

---

## less

Best for large files.

```bash
less log.txt
```

Features:

- Scroll up/down
- Search text
- Efficient memory usage

---

## head

First lines:

```bash
head file.txt
```

---

## tail

Last lines:

```bash
tail file.txt
```

Live monitoring:

```bash
tail -f application.log
```

Visual:

```text
File
│
├── First lines ← head
│
│
│
├── Middle
│
│
└── Last lines ← tail
```

---

# Understanding File Metadata

Linux stores information about files.

```bash
stat file.txt
```

Shows:

- Size
- Permissions
- Owner
- Access time
- Modification time
- Inode

Visual:

```text
file.txt
│
├── Owner
├── Group
├── Size
├── Permissions
├── Created
└── Modified
```

---

# File Type Detection

## file command

```bash
file image.png
```

Output:

```text
PNG image data
```

Useful when extensions are misleading.

Example:

```bash
file mysteryfile
```

Linux inspects contents instead of extension.

---

# Searching Files

One of the most important Linux skills.

---

## find

Search filesystem.

```bash
find . -name "*.txt"
```

Visual:

```text
project
│
├── notes.txt
├── report.pdf
└── docs
    └── readme.txt
```

Result:

```text
./notes.txt
./docs/readme.txt
```

---

## locate

Database-based search.

```bash
locate nginx.conf
```

Much faster than find.

---

## tree

Visual directory structure.

```bash
tree
```

Output:

```text
project
├── src
├── docs
└── tests
```

---

# Wildcards

Wildcards allow matching patterns.

---

## *

Match everything.

```bash
ls *.txt
```

Matches:

```text
a.txt
b.txt
c.txt
```

---

## ?

Match one character.

```bash
ls file?.txt
```

Matches:

```text
file1.txt
file2.txt
```

Not:

```text
file10.txt
```

---

## []

Character sets.

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

# Redirection

Control command output.

Visual:

```text
Command
   │
   ▼
Output
   │
   ▼
Terminal
```

Can redirect:

```text
Command
   │
   ▼
File
```

---

## >

Overwrite file

```bash
echo Hello > output.txt
```

---

## >>

Append

```bash
echo World >> output.txt
```

---

## <

Input redirection

```bash
sort < names.txt
```

---

# Pipes

Pipes connect commands.

Symbol:

```bash
|
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

Example:

```bash
cat access.log | grep ERROR
```

Flow:

```text
access.log
      │
      ▼
     cat
      │
      ▼
    grep
      │
      ▼
  ERROR lines
```

---

# Real-World Examples

## Count Log Errors

```bash
grep ERROR app.log | wc -l
```

---

## Backup Project

```bash
cp -r project project_backup
```

---

## Monitor Live Logs

```bash
tail -f server.log
```

---

## Find Configuration Files

```bash
find /etc -name "*.conf"
```

---

## Delete Temporary Files

```bash
find . -name "*.tmp" -delete
```

---

# Common Beginner Mistakes

## Accidentally Deleting Files

```bash
rm important.txt
```

No recycle bin.

---

## Forgetting Recursive Copy

Wrong:

```bash
cp project backup
```

Correct:

```bash
cp -r project backup
```

---

## Using Relative Paths Incorrectly

Wrong directory:

```bash
cd project
```

Fails if path doesn't exist.

Always verify with:

```bash
pwd
```

---

# Skills You Will Gain

After completing this module:

```text
✓ Navigate Linux filesystem
✓ Create files/directories
✓ Copy and move data
✓ Delete safely
✓ View file contents
✓ Search efficiently
✓ Understand metadata
✓ Use wildcards
✓ Use redirection
✓ Use pipes
✓ Troubleshoot filesystem issues
✓ Perform admin tasks
```

---

# Learning Order

Study files in this exact order:

```text
1. pwd.md
2. ls.md
3. cd.md

4. mkdir.md
5. rmdir.md
6. touch.md

7. cp.md
8. mv.md
9. rm.md

10. cat.md
11. less.md
12. head-tail.md

13. stat.md
14. file-command.md

15. find.md
16. locate.md
17. tree.md

18. wildcard-expansion.md
19. redirection.md
20. pipes.md

21. interview-questions.md
22. references.md
```

---

# Module Completion Checklist

- [ ] I can navigate directories using pwd, ls, and cd
- [ ] I can create files and directories
- [ ] I can copy and move files
- [ ] I can safely delete files
- [ ] I can inspect file metadata
- [ ] I can search files using find and locate
- [ ] I understand wildcard expansion
- [ ] I can use redirection operators
- [ ] I can connect commands using pipes
- [ ] I can solve practical Linux file management tasks
- [ ] I can answer Linux interview questions confidently

---

# What Comes Next?

After mastering file operations:

```text
05-users-and-groups/
│
├── User Management
├── Group Management
├── User Databases
├── Password Management
├── sudo
├── Authentication
└── Security Basics
```

You will then learn how Linux controls access to files and directories through users, groups, permissions, and ownership.
