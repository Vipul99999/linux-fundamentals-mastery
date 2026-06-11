# head and tail Commands

# Why Do head and tail Exist?

Imagine a book with:

```text
1000 pages
```

Most of the time you don't want to read:

```text
Page 1 → Page 1000
```

You usually want:

```text
Beginning of book
or
End of book
```

Linux files can be huge:

```text
app.log
database.log
access.log
users.csv
```

Some may contain:

```text
10,000 lines
100,000 lines
1,000,000+ lines
```

Reading everything is inefficient.

Linux provides:

```text
head → Show beginning

tail → Show end
```

---

# Real-Life Analogy

Imagine a newspaper.

Usually you read:

```text
Headline
```

before deciding to read everything.

Linux equivalent:

```bash
head news.txt
```

Similarly, if you want the latest news:

```text
Last page
```

Linux equivalent:

```bash
tail news.txt
```

---

# Visual Overview

```text
Huge File

┌─────────────────┐
│ Line 1          │ ← head
│ Line 2          │
│ Line 3          │
│ ...             │
│ ...             │
│ ...             │
│ Line 9998       │
│ Line 9999       │
│ Line 10000      │ ← tail
└─────────────────┘
```

---

# Part 1: head Command

# What is head?

`head` displays the beginning of a file.

Default:

```text
First 10 lines
```

---

# Syntax

```bash
head filename
```

Example:

```bash
head notes.txt
```

---

# Example

File:

```text
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
Line 11
Line 12
```

Command:

```bash
head notes.txt
```

Output:

```text
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
```

---

# Visual

```text
notes.txt

┌───────────────┐
│ Line 1        │ ← shown
│ Line 2        │ ← shown
│ Line 3        │ ← shown
│ ...           │
│ Line 10       │ ← shown
│───────────────│
│ Line 11       │ hidden
│ Line 12       │ hidden
└───────────────┘
```

---

# Specify Number of Lines

Option:

```bash
head -n 5 notes.txt
```

Output:

```text
Line 1
Line 2
Line 3
Line 4
Line 5
```

---

# Visual

```text
File

Line 1 ← shown
Line 2 ← shown
Line 3 ← shown
Line 4 ← shown
Line 5 ← shown
────────────
Line 6
Line 7
...
```

---

# Show First Line Only

```bash
head -n 1 file.txt
```

Useful for:

```text
CSV Headers
Configuration Names
File Verification
```

---

# Common Uses of head

## Inspect CSV Files

```bash
head users.csv
```

Output:

```text
id,name,email
1,Alice,...
2,Bob,...
```

---

## Check Log Structure

```bash
head server.log
```

See how logs begin.

---

## Verify Data Files

```bash
head dataset.csv
```

Before processing huge datasets.

---

# Part 2: tail Command

# What is tail?

`tail` displays the end of a file.

Default:

```text
Last 10 lines
```

---

# Syntax

```bash
tail filename
```

Example:

```bash
tail server.log
```

---

# Example

File:

```text
Line 1
Line 2
...
Line 100
```

Command:

```bash
tail file.txt
```

Output:

```text
Line 91
Line 92
Line 93
Line 94
Line 95
Line 96
Line 97
Line 98
Line 99
Line 100
```

---

# Visual

```text
file.txt

Line 1
Line 2
...
Line 90

──────────────
Line 91 ← shown
Line 92 ← shown
Line 93 ← shown
...
Line 100 ← shown
```

---

# Show Specific Number Of Lines

```bash
tail -n 5 file.txt
```

Output:

```text
Line 96
Line 97
Line 98
Line 99
Line 100
```

---

# Why tail Is More Popular Than head

In real systems:

```text
New logs are appended to the end.
```

Example:

```text
server.log

Old Logs
Old Logs
Old Logs
NEW ERROR
NEW ERROR
NEW ERROR
```

Most recent information is at the bottom.

Therefore:

```bash
tail server.log
```

is used constantly.

---

# Visual

```text
Log File

Start
 │
 │
 │
 ▼
Old Events

 ▼
Recent Events

 ▼
Latest Errors
```

tail focuses on:

```text
Latest Events
```

---

# tail -f (Follow Mode)

One of the most important Linux commands.

---

# What Is tail -f?

The `-f` means:

```text
Follow
```

Linux keeps watching the file.

---

# Syntax

```bash
tail -f server.log
```

---

# Visual

Normal tail:

```text
Read File
   │
   ▼
Show Last Lines
   │
   ▼
Exit
```

---

tail -f:

```text
Read File
   │
   ▼
Show Last Lines
   │
   ▼
Keep Watching
   │
   ▼
Show New Lines
```

---

# Real-Time Log Monitoring

Imagine application logs:

```text
INFO Started
INFO Connected
```

Run:

```bash
tail -f app.log
```

Later application writes:

```text
ERROR Database Failed
```

Immediately appears on screen.

---

# Visual Timeline

```text
Time 1

INFO Started

↓

Time 2

INFO Connected

↓

Time 3

ERROR Database Failed

↓

tail -f shows everything live
```

---

# Why Administrators Love tail -f

Monitoring:

```text
Web Servers
Databases
Applications
Containers
Microservices
Security Logs
```

Common command:

```bash
tail -f /var/log/syslog
```

or

```bash
tail -f application.log
```

---

# Exit tail -f

Press:

```text
CTRL + C
```

---

# head vs tail

| Feature        | head      | tail       |
| -------------- | --------- | ---------- |
| Shows          | Beginning | End        |
| Default Lines  | 10        | 10         |
| Log Monitoring | No        | Yes        |
| Follow Updates | No        | Yes (`-f`) |
| CSV Inspection | Excellent | Poor       |
| Recent Errors  | Poor      | Excellent  |

---

# Visual Comparison

```text
Large File

┌─────────────────────┐
│ Line 1              │ ← head
│ Line 2              │
│ Line 3              │
│ ...                 │
│ ...                 │
│ ...                 │
│ Line 9998           │
│ Line 9999           │
│ Line 10000          │ ← tail
└─────────────────────┘
```

---

# Internal Working

# head

```text
Open File
    │
    ▼
Read First N Lines
    │
    ▼
Stop
```

---

# tail

```text
Open File
    │
    ▼
Jump Near End
    │
    ▼
Read Last N Lines
```

---

# tail -f

```text
Open File
    │
    ▼
Read Last N Lines
    │
    ▼
Watch For Changes
    │
    ▼
Display New Lines
```

---

# Real-World Examples

## Check CSV Header

```bash
head -n 1 users.csv
```

---

## View First 20 Lines

```bash
head -n 20 config.txt
```

---

## View Last 50 Log Entries

```bash
tail -n 50 app.log
```

---

## Monitor Live Logs

```bash
tail -f app.log
```

---

## Debug Errors

```bash
tail -100 server.log
```

Review recent events before a crash.

---

# Hands-On Lab

Create:

```bash
seq 1 100 > numbers.txt
```

---

View beginning:

```bash
head numbers.txt
```

Expected:

```text
1
2
3
...
10
```

---

View first 5:

```bash
head -n 5 numbers.txt
```

Expected:

```text
1
2
3
4
5
```

---

View end:

```bash
tail numbers.txt
```

Expected:

```text
91
92
...
100
```

---

View last 5:

```bash
tail -n 5 numbers.txt
```

Expected:

```text
96
97
98
99
100
```

---

# Common Beginner Mistakes

## Using cat For Huge Logs

Bad:

```bash
cat huge.log
```

Better:

```bash
tail huge.log
```

---

## Forgetting CTRL+C

When using:

```bash
tail -f file.log
```

Press:

```text
CTRL + C
```

to exit.

---

## Confusing head and tail

Remember:

```text
head
↑
Top

tail
↓
Bottom
```

---

# Memory Trick

Think of an animal.

```text
HEAD
 │
 ▼
Beginning

TAIL
 │
 ▼
End
```

Linux uses the same idea.

---

# Visual Memory Map

```text
File

HEAD
 │
 ▼
Beginning

...
...
...

TAIL
 │
 ▼
End
```

---

# Interview Questions

## What does head do?

Displays the beginning of a file.

---

## What does tail do?

Displays the end of a file.

---

## Default number of lines shown?

```text
10
```

for both commands.

---

## How do you show first 20 lines?

```bash
head -n 20 file.txt
```

---

## How do you show last 50 lines?

```bash
tail -n 50 file.txt
```

---

## What does tail -f do?

Continuously monitors a file and displays new content as it is added.

---

## How do you stop tail -f?

```text
CTRL + C
```

---

# Quick Summary

```text
head
│
└── Beginning Of File

tail
│
└── End Of File

tail -f
│
└── Live Monitoring
```

---

# Learning Flow

```text
touch
│
└── Create File

cat
│
└── Read Entire File

less
│
└── Read Large File

head
│
└── Read Beginning

tail
│
└── Read End

tail -f
│
└── Watch Live Changes
```
