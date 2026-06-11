# interview-questions.md

> This file is designed for:
>
> ```text
> Linux Beginners
> Linux Administrators
> DevOps Engineers
> SRE Engineers
> Cloud Engineers
> Security Engineers
> System Design Interviews
> Production Troubleshooting Interviews
> ```
>
> The goal is not memorization.
>
> The goal is understanding:
>
> ```text
> Why commands exist
> How they work internally
> When to use them
> When NOT to use them
> ```

---

# 04 - File Operations Interview Questions

# Learning Roadmap

```text
Level 1
│
├── Basic Concepts
│
Level 2
│
├── Daily Linux Usage
│
Level 3
│
├── Advanced File Operations
│
Level 4
│
├── DevOps & Cloud
│
Level 5
│
├── Security
│
Level 6
│
└── Production Scenarios
```

---

# Section 1: Basic Questions

## Q1. What is the purpose of pwd?

### Answer

Displays the current working directory.

Example:

```bash
pwd
```

Output:

```text
/home/vip/projects
```

Visual:

```text
Filesystem
│
└── home
    │
    └── vip
        │
        └── projects  ← Current Location
```

---

## Q2. Difference between absolute path and relative path?

### Absolute Path

Starts from root.

```bash
/home/user/docs
```

Visual:

```text
/
│
└── home
    │
    └── user
        │
        └── docs
```

---

### Relative Path

Starts from current location.

```bash
docs
```

Visual:

```text
Current Directory
│
└── docs
```

---

## Q3. What does ls do?

### Answer

Lists directory contents.

Example:

```bash
ls
```

Output:

```text
README.md
src
docs
```

---

## Q4. Difference between ls and tree?

| Feature          | ls      | tree      |
| ---------------- | ------- | --------- |
| Show Files       | Yes     | Yes       |
| Show Folders     | Yes     | Yes       |
| Show Hierarchy   | No      | Yes       |
| Visual Structure | Limited | Excellent |

---

Visual:

```text
ls

src
docs
README.md
```

```text
tree

project
├── src
├── docs
└── README.md
```

---

## Q5. What command creates a directory?

```bash
mkdir project
```

---

## Q6. What command creates an empty file?

```bash
touch notes.txt
```

---

## Q7. Difference between file and directory?

### File

Stores data.

```text
notes.txt
```

### Directory

Stores references to files and directories.

```text
docs/
```

Visual:

```text
Directory
│
├── file1.txt
├── file2.txt
└── file3.txt
```

---

# Section 2: File Manipulation

## Q8. Difference between cp and mv?

### cp

Copies.

```bash
cp file.txt backup/
```

Visual:

```text
Before

file.txt

After

file.txt
backup/file.txt
```

---

### mv

Moves or renames.

```bash
mv file.txt backup/
```

Visual:

```text
Before

file.txt

After

backup/file.txt
```

Original disappears.

---

## Q9. How do you rename a file?

```bash
mv old.txt new.txt
```

---

## Q10. How do you remove a file?

```bash
rm file.txt
```

---

## Q11. How do you remove an empty directory?

```bash
rmdir empty-dir
```

---

## Q12. How do you remove a non-empty directory?

```bash
rm -r project
```

---

## Q13. Why is rm -rf dangerous?

Because it recursively deletes data without confirmation.

Visual:

```text
rm -rf project

project
│
├── src
├── docs
└── data

↓

Everything Deleted
```

---

# Section 3: Viewing File Content

## Q14. Difference between cat and less?

### cat

Displays everything immediately.

```bash
cat file.txt
```

---

### less

Displays incrementally.

```bash
less file.txt
```

Useful for large files.

---

## Q15. When should you use less instead of cat?

Large logs.

Example:

```bash
less application.log
```

---

## Q16. Difference between head and tail?

### head

Beginning of file.

```bash
head file.txt
```

---

### tail

End of file.

```bash
tail file.txt
```

---

## Q17. How do you monitor a log file in real time?

```bash
tail -f app.log
```

Visual:

```text
Application
     │
     ▼
app.log
     │
     ▼
tail -f
```

---

# Section 4: Metadata

## Q18. What does stat show?

Displays metadata.

Example:

```bash
stat file.txt
```

Shows:

```text
Size
Permissions
Owner
Timestamps
```

---

## Q19. Difference between stat and ls -l?

### ls -l

Summary.

### stat

Detailed metadata.

---

# Section 5: Searching Files

## Q20. What is find?

Real-time filesystem search tool.

Example:

```bash
find . -name "*.txt"
```

---

## Q21. What is locate?

Database-based file search tool.

Example:

```bash
locate notes.txt
```

---

## Q22. Difference between find and locate?

| Feature         | find | locate     |
| --------------- | ---- | ---------- |
| Real Time       | Yes  | No         |
| Database        | No   | Yes        |
| Fast            | No   | Yes        |
| Always Accurate | Yes  | Not Always |

---

Visual:

```text
find
│
└── Search Filesystem

locate
│
└── Search Database
```

---

## Q23. Why can locate miss files?

Because its database may be outdated.

Solution:

```bash
sudo updatedb
```

---

## Q24. How do you find all log files?

```bash
find . -name "*.log"
```

---

## Q25. How do you find files larger than 1GB?

```bash
find . -size +1G
```

---

## Q26. How do you find recently modified files?

```bash
find . -mtime -1
```

---

## Q27. How do you find directories only?

```bash
find . -type d
```

---

# Section 6: Tree & Visualization

## Q28. Why is tree useful?

Provides hierarchical visualization.

Visual:

```text
project
├── src
├── tests
└── docs
```

---

## Q29. How do you show hidden files with tree?

```bash
tree -a
```

---

## Q30. How do you limit tree depth?

```bash
tree -L 2
```

---

# Section 7: Wildcards

## Q31. What is wildcard expansion?

Pattern matching performed by the shell.

---

## Q32. Who expands wildcards?

### Correct Answer

Shell.

Not:

```text
ls
cp
mv
rm
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

file1.txt file2.txt file3.txt

 │
 ▼

Command
```

---

## Q33. What does * mean?

Match zero or more characters.

Example:

```bash
*.txt
```

---

## Q34. What does ? mean?

Match exactly one character.

Example:

```bash
file?.txt
```

---

## Q35. What does [a-z] mean?

Match lowercase letters.

---

## Q36. Difference between wildcard and regex?

| Wildcard          | Regex         |
| ----------------- | ------------- |
| Filename Matching | Text Matching |
| Simpler           | More Powerful |

---

# Section 8: Redirection

## Q37. What are the three standard streams?

```text
stdin
stdout
stderr
```

---

## Q38. File descriptors?

| Stream | FD |
| ------ | -- |
| stdin  | 0  |
| stdout | 1  |
| stderr | 2  |

---

## Q39. Difference between > and >>?

### >

Overwrite.

### >>

Append.

---

Visual:

```text
>

Replace File

>>

Add To End
```

---

## Q40. What does 2> do?

Redirect stderr.

Example:

```bash
ls missing-file 2> error.log
```

---

## Q41. What is /dev/null?

Special file that discards data.

Visual:

```text
Output
 │
 ▼

/dev/null

 │
 ▼

Gone Forever
```

---

## Q42. How do you redirect both stdout and stderr?

```bash
command > output.log 2>&1
```

or

```bash
command &> output.log
```

---

# Section 9: Pipes

## Q43. What is a pipe?

Connects stdout of one command to stdin of another.

---

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

## Q44. What symbol represents a pipe?

```bash
|
```

---

## Q45. Difference between pipe and redirection?

### Pipe

```text
Command → Command
```

### Redirection

```text
Command → File
```

---

## Q46. What does this command do?

```bash
ls | wc -l
```

Counts items produced by ls.

---

## Q47. Do pipes transfer stderr?

No.

Only stdout unless stderr is redirected.

---

# Section 10: Intermediate Questions

## Q48. What happens internally when you run?

```bash
ls *.txt
```

### Answer

Shell expands:

```text
*.txt
```

into:

```text
file1.txt file2.txt file3.txt
```

before ls executes.

---

## Q49. What happens internally when you run?

```bash
cat file.txt | grep error
```

Visual:

```text
file.txt
    │
    ▼

cat
    │
    ▼

grep error
    │
    ▼

Output
```

---

## Q50. Why are pipes preferred over temporary files?

Because:

```text
Less Disk I/O
Less Storage
More Efficient
Cleaner Workflows
```

---

# Section 11: DevOps Questions

## Q51. How do you find Dockerfiles?

```bash
find . -name Dockerfile
```

---

## Q52. How do you locate Kubernetes manifests?

```bash
find . -name "*.yaml"
```

---

## Q53. How do you find environment files?

```bash
find . -name ".env*"
```

---

## Q54. How do you find build artifacts?

```bash
find . -name "*.jar"
find . -name "*.war"
```

---

## Q55. How do you save application logs?

```bash
node app.js > app.log
```

---

## Q56. How do you separate normal logs and error logs?

```bash
node app.js > output.log 2> error.log
```

---

# Section 12: Security Questions

## Q57. How do you find world-writable files?

```bash
find / -perm -002
```

---

## Q58. How do you find SUID files?

```bash
find / -perm -4000
```

---

## Q59. How do you locate private keys?

```bash
find /etc -name "*.key"
```

---

## Q60. Why might hidden files matter?

They may contain:

```text
Configuration
Application Settings
User Preferences
Security Information
```

---

# Section 13: Production Scenarios

## Q61. Server disk is full. What do you do first?

Search large files:

```bash
find / -size +1G
```

Visual:

```text
Disk Full
    │
    ▼

Find Large Files
    │
    ▼

Investigate
```

---

## Q62. Application crashes. How do you inspect logs?

```bash
tail -f app.log
```

or

```bash
less app.log
```

---

## Q63. User says a file disappeared.

What should you check?

```text
Wrong Directory
Moved File
Renamed File
Deleted File
Permissions
```

Search:

```bash
find / -name filename
```

---

## Q64. Locate is not finding a newly created file.

Why?

Database outdated.

Run:

```bash
sudo updatedb
```

---

## Q65. CI/CD pipeline failed. Where do you start?

Check:

```text
Build Logs
Error Logs
Artifacts
Configuration Files
```

Commands:

```bash
less build.log

grep ERROR build.log
```

---

## Q66. Why is this dangerous?

```bash
rm -rf *
```

Visual:

```text
Current Directory
      │
      ▼
Everything Deleted
```

---

## Q67. How would you count all log files?

```bash
find . -name "*.log" | wc -l
```

---

## Q68. How would you count ERROR messages?

```bash
grep ERROR app.log | wc -l
```

---

## Q69. Why is this better?

```bash
ps aux | grep nginx
```

than manually reading all processes?

Because filtering reduces noise.

---

## Q70. Explain Linux file operations in one diagram.

```text
Filesystem
     │
     ▼

Navigation
│
├── pwd
├── cd
└── ls

     │
     ▼

Manipulation
│
├── touch
├── cp
├── mv
├── rm
└── mkdir

     │
     ▼

Inspection
│
├── cat
├── less
├── head
├── tail
└── stat

     │
     ▼

Searching
│
├── find
├── locate
└── tree

     │
     ▼

Automation
│
├── Wildcards
├── Redirection
└── Pipes
```

---

# Rapid Revision Sheet

```text
pwd      → Current Directory

ls       → List Files

cd       → Change Directory

mkdir    → Create Directory

rmdir    → Remove Empty Directory

touch    → Create File

cp       → Copy

mv       → Move / Rename

rm       → Delete

cat      → Display File

less     → View Large Files

head     → Beginning

tail     → End

stat     → Metadata

find     → Real-Time Search

locate   → Indexed Search

tree     → Visual Structure

*        → Many Characters

?        → One Character

>        → Overwrite

>>       → Append

<        → Input

2>       → Error Output

|        → Command Pipeline
```

---

# Master Concept

```text
Linux File Operations
│
├── Navigate
├── Create
├── Copy
├── Move
├── Delete
├── View
├── Search
├── Visualize
├── Redirect
└── Automate
```

If a learner can confidently answer these 70 questions and explain the visuals, they have a strong foundation in Linux file operations for interviews, administration, DevOps, and troubleshooting.
