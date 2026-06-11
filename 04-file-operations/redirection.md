# Redirection

redirection.md is one of the most important files in the entire Linux course.

Why?

Because after this chapter users stop thinking:

```
Linux = Commands

```
and start understanding:

```
Linux = Data Flow
```

---

Everything in Linux revolves around:
```
Input
 ↓
Processing
 ↓
Output
```
---

Redirection is the foundation of:
```
Shell Scripting
Automation
DevOps
Logging
CI/CD
Monitoring
Data Processing
System Administration
```

# Input, Output & Redirection

# Introduction

Imagine a water system.

```text
Water Source
     │
     ▼
Pipe
     │
     ▼
Destination
```

Linux commands work similarly.

```text
Input
  │
  ▼
Command
  │
  ▼
Output
```

This flow is called:

```text
Input/Output (I/O)
```

Redirection allows us to control where data comes from and where it goes.

---

# Why Redirection Exists

Normally:

```bash
echo "Hello Linux"
```

Output appears on the screen.

```text
Hello Linux
```

But what if we want:

```text
Save Output To File

Store Logs

Generate Reports

Capture Errors

Feed Input Automatically
```

That's why redirection exists.

---

# The Three Standard Streams

Every Linux process automatically gets:

```text
Standard Input
Standard Output
Standard Error
```

Visual:

```text
            ┌───────────┐
Input ─────►│ Command   │─────► Output
            └───────────┘
                    │
                    ▼
                 Errors
```

---

# Standard Input (stdin)

File Descriptor:

```text
0
```

Purpose:

```text
Receives Input
```

Visual:

```text
Keyboard
   │
   ▼
stdin (0)
   │
   ▼
Command
```

---

# Standard Output (stdout)

File Descriptor:

```text
1
```

Purpose:

```text
Normal Output
```

Visual:

```text
Command
   │
   ▼
stdout (1)
   │
   ▼
Terminal
```

---

# Standard Error (stderr)

File Descriptor:

```text
2
```

Purpose:

```text
Error Messages
```

Visual:

```text
Command
   │
   ▼
stderr (2)
   │
   ▼
Terminal
```

---

# The Big Picture

```text
                 stdin (0)
                      │
                      ▼

              ┌────────────┐
              │  Command   │
              └────────────┘

                │       │
                ▼       ▼

         stdout(1)  stderr(2)
```

---

# Understanding File Descriptors

Linux internally uses numbers.

| Stream | Number |
| ------ | ------ |
| stdin  | 0      |
| stdout | 1      |
| stderr | 2      |

Remember:

```text
0 = Input

1 = Output

2 = Error
```

---

# Output Redirection (>)

Most common redirection.

Example:

```bash
echo "Hello Linux" > notes.txt
```

Visual:

```text
echo
 │
 ▼
stdout
 │
 ▼
notes.txt
```

Result:

```text
notes.txt

Hello Linux
```

---

# Internal Flow

```text
echo "Hello Linux"
        │
        ▼
stdout
        │
        ▼
notes.txt
```

Instead of:

```text
Screen
```

---

# Important Warning

`>` overwrites files.

Example:

```bash
echo "First" > notes.txt
```

File:

```text
First
```

Then:

```bash
echo "Second" > notes.txt
```

File becomes:

```text
Second
```

Old content is lost.

---

# Visual

```text
Before

notes.txt
│
└── First

After

notes.txt
│
└── Second
```

---

# Append Redirection (>>)

Append means:

```text
Add To End
```

Example:

```bash
echo "Hello" > notes.txt

echo "Linux" >> notes.txt
```

Result:

```text
Hello
Linux
```

---

# Visual

```text
notes.txt

Hello
      │
      ▼
Append
      │
      ▼

Hello
Linux
```

---

# Comparison

```text
>

Overwrite


>>

Append
```

---

# Redirecting Command Output

Example:

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

# Save Process List

```bash
ps aux > processes.txt
```

Useful for:

```text
Troubleshooting
Auditing
Reporting
```

---

# Save Directory Tree

```bash
tree > structure.txt
```

Useful for documentation.

---

# Redirecting Errors

Suppose:

```bash
ls missing-file
```

Output:

```text
No such file or directory
```

This is:

```text
stderr
```

not stdout.

---

# Error Redirection

```bash
ls missing-file 2> errors.txt
```

Visual:

```text
Command
   │
   ▼
stderr
   │
   ▼
errors.txt
```

Result:

```text
errors.txt
│
└── No such file or directory
```

---

# Why 2?

Remember:

```text
stderr = 2
```

Therefore:

```bash
2>
```

means:

```text
Redirect Errors
```

---

# Visual

```text
stdout = 1

stderr = 2
```

---

# Redirect Normal Output Only

```bash
ls > output.txt
```

Errors still appear on screen.

---

# Redirect Errors Only

```bash
ls missing-file 2> errors.txt
```

Normal output still appears on screen.

---

# Redirect Both Output And Errors

```bash
command > output.txt 2>&1
```

Visual:

```text
stdout
   │
   ▼

output.txt

stderr
   │
   ▼

output.txt
```

Everything goes to one file.

---

# Modern Alternative

```bash
command &> output.txt
```

Same result.

---

# Visual

```text
stdout
      ┐
      │
      ▼

output.txt

      ▲
      │
stderr
```

---

# Discard Output

Linux provides:

```text
/dev/null
```

Think of it as:

```text
Black Hole
```

---

# Visual

```text
Output
   │
   ▼

/dev/null

   │
   ▼

Disappears Forever
```

---

# Example

```bash
ls > /dev/null
```

Output discarded.

---

# Discard Errors

```bash
ls missing-file 2>/dev/null
```

Error hidden.

---

# Discard Everything

```bash
command > /dev/null 2>&1
```

Visual:

```text
stdout ──► /dev/null

stderr ──► /dev/null
```

---

# Input Redirection (<)

Instead of keyboard input.

Example:

File:

```text
names.txt

Alice
Bob
Charlie
```

Command:

```bash
wc -l < names.txt
```

Visual:

```text
names.txt
    │
    ▼
stdin
    │
    ▼
wc -l
```

Output:

```text
3
```

---

# Internal Flow

```text
File
 │
 ▼
stdin
 │
 ▼
Command
```

---

# Here Documents (<<)

Provide multi-line input.

Example:

```bash
cat << EOF
Hello
Linux
EOF
```

Output:

```text
Hello
Linux
```

---

# Visual

```text
Multi-Line Text
      │
      ▼
stdin
      │
      ▼
cat
```

---

# Here Strings (<<<)

Single-line input.

```bash
wc -w <<< "Hello Linux World"
```

Output:

```text
3
```

---

# Visual

```text
String
  │
  ▼
stdin
  │
  ▼
Command
```

---

# Logging in DevOps

Application logs:

```bash
node app.js > app.log
```

Visual:

```text
Application
     │
     ▼
app.log
```

---

# Error Logging

```bash
node app.js 2> errors.log
```

Visual:

```text
Application Errors
       │
       ▼
errors.log
```

---

# Separate Logs

```bash
node app.js > output.log 2> error.log
```

Visual:

```text
stdout ──► output.log

stderr ──► error.log
```

---

# CI/CD Usage

Build output:

```bash
npm run build > build.log
```

Test output:

```bash
npm test > test.log
```

Common in automation pipelines.

---

# Security Auditing

Save findings:

```bash
find /etc -name "*.conf" > audit.txt
```

Visual:

```text
Audit Results
      │
      ▼
audit.txt
```

---

# Monitoring Example

Save disk usage:

```bash
df -h > disk-report.txt
```

Visual:

```text
System Report
      │
      ▼
disk-report.txt
```

---

# Common Mistakes

## Accidentally Overwriting Files

Dangerous:

```bash
>
```

Always remember:

```text
>
=
Replace
```

---

## Forgetting stderr

Example:

```bash
ls missing-file > output.txt
```

Error still appears.

Need:

```bash
ls missing-file 2> errors.txt
```

---

## Confusing > and >>

```text
>

Overwrite


>>

Append
```

---

# Memory Trick

Think:

```text
>

Replace


>>

Add


<

Read


2>

Errors
```

---

# Visual Cheat Sheet

```text
stdout
 │
 ├── >
 │
 └── >>

stderr
 │
 └── 2>

stdin
 │
 └── <

Everything
 │
 └── &>
```

---

# Real Data Flow Diagram

```text
Keyboard
    │
    ▼

stdin (0)
    │
    ▼

┌──────────┐
│ Command  │
└──────────┘

    │
    ├──────────► stdout (1)
    │
    └──────────► stderr (2)
```

---

# Hands-On Lab

Create file:

```bash
echo "Linux" > notes.txt
```

Append:

```bash
echo "Fundamentals" >> notes.txt
```

View:

```bash
cat notes.txt
```

Output:

```text
Linux
Fundamentals
```

---

Redirect errors:

```bash
ls missing-file 2> error.log
```

Check:

```bash
cat error.log
```

---

Input redirection:

```bash
wc -l < notes.txt
```

Observe output.

---

# Interview Questions

### What are the three standard streams?

```text
stdin
stdout
stderr
```

---

### What is stdout?

Standard Output (File Descriptor 1).

---

### What is stderr?

Standard Error (File Descriptor 2).

---

### Difference between > and >>?

```text
>

Overwrite


>>

Append
```

---

### What does 2> mean?

Redirect stderr.

---

### What is /dev/null?

A special file that discards data.

---

### What does < do?

Redirect input into a command.

---

# Quick Cheat Sheet

```bash
>

>>

<

2>

2>>

&>

command > file

command >> file

command < file

command 2> errors.log

command > output.log 2>&1

command &> all.log

command > /dev/null

command > /dev/null 2>&1
```

# Key Takeaway

```text
Linux Commands
        │
        ▼
Data Flows Through Streams

stdin  (0)
stdout (1)
stderr (2)

Redirection Controls
Where Data Comes From
And
Where Data Goes
```
