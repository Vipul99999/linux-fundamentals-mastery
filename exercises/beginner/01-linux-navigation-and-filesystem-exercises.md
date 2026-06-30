# Linux Navigation & Filesystem Exercises

> Beginner Track — Exercise 01

---

# Why This Exercise Exists

Most Linux beginners immediately start memorizing commands:

```bash
ls
cd
pwd
mkdir
rm
```

This is a mistake.

Professional Linux engineers do not think in commands.

They think in:

* Filesystems
* Directory hierarchies
* Resource locations
* Data movement
* System organization

The purpose of this exercise is to build the mental model that every Linux system is fundamentally a filesystem tree.

Before learning Docker, Kubernetes, databases, cloud infrastructure, or DevOps, you must understand how Linux organizes information.

Everything else is built on top of this foundation.

---

# Problem This Exercise Solves

New engineers often:

* Get lost in directories
* Use relative paths incorrectly
* Delete wrong files
* Break deployments
* Misconfigure applications
* Struggle with Docker volumes
* Struggle with Kubernetes mounts
* Misunderstand Linux paths

Example production issue:

A developer deploys an application.

Configuration expected:

```bash
/etc/myapp/config.yaml
```

Actual configuration:

```bash
/home/user/config.yaml
```

Application fails.

Root cause?

Poor filesystem understanding.

---

# Learning Goals

After completing these exercises, you should understand:

✓ What a filesystem tree is

✓ How Linux organizes directories

✓ Absolute paths

✓ Relative paths

✓ Navigation commands

✓ File discovery

✓ Directory hierarchy intuition

✓ Real-world system layouts

---

# Mental Model

Imagine Linux as a giant city.

```text
Linux System
│
├── /home      -> Residential Area
├── /etc       -> Government Records
├── /var       -> Business Activity
├── /tmp       -> Temporary Market
├── /usr       -> Public Library
├── /bin       -> Essential Tools
└── /dev       -> Hardware District
```

Every file has an address.

Just like:

```text
House:
221B Baker Street

Linux:
/home/vip/projects/app.js
```

The filesystem is a map.

Commands merely help you move through it.

---

# Filesystem Visualization

```text
/
├── home
│   └── vip
│       ├── documents
│       ├── downloads
│       └── projects
│
├── etc
│   ├── passwd
│   ├── hosts
│   └── ssh
│
├── var
│   ├── log
│   └── lib
│
└── tmp
```

---

# Lab Setup

Create a practice environment.

```bash
mkdir -p ~/linux-lab
cd ~/linux-lab
```

Create directories:

```bash
mkdir projects
mkdir documents
mkdir downloads
mkdir backups
```

Create files:

```bash
touch projects/app.py
touch projects/server.py
touch documents/notes.txt
touch downloads/image.jpg
touch backups/archive.tar
```

Verify:

```bash
tree .
```

If tree is unavailable:

```bash
find .
```

---

# Exercise 1 — Discover Your Location

### Objective

Understand where you are.

### Tasks

1. Print current directory.
2. Move to your home directory.
3. Print directory again.
4. Move back to linux-lab.
5. Verify location.

### Commands To Use

```bash
pwd
cd
```

### Expected Understanding

Linux always tracks:

```text
Current Working Directory (CWD)
```

Many commands operate relative to this location.

---

# Exercise 2 — Explore Directory Contents

### Objective

Learn how Linux reveals information.

### Tasks

List contents using:

```bash
ls
ls -l
ls -a
ls -lh
```

Answer:

1. What changes between outputs?
2. Which files are hidden?
3. Which view is easiest to read?

### Engineering Insight

Production troubleshooting often begins with:

```bash
ls -lah
```

It provides:

* Ownership
* Permissions
* Size
* Hidden files

---

# Exercise 3 — Absolute Paths

### Objective

Understand exact locations.

Navigate using full paths.

Examples:

```bash
cd /home
cd /home/$USER
cd /home/$USER/linux-lab
```

Questions:

1. Why does this work from anywhere?
2. What is the difference between:

```bash
/home/user
```

and

```bash
user
```

---

# Exercise 4 — Relative Paths

### Objective

Understand path relationships.

Start here:

```bash
~/linux-lab
```

Move into:

```bash
projects
```

Without using absolute paths:

1. Go to documents.
2. Return to linux-lab.
3. Enter downloads.
4. Return home.

Use:

```bash
..
.
```

---

# Exercise 5 — Create Nested Directories

Create:

```text
company
├── engineering
│   ├── backend
│   ├── frontend
│   └── devops
└── hr
```

Command goal:

```bash
mkdir -p
```

Verify structure.

---

# Exercise 6 — Create Project Structure

Build:

```text
web-app
├── src
├── tests
├── docs
├── config
└── scripts
```

This mirrors real software projects.

---

# Exercise 7 — Find Files

Search for:

```bash
notes.txt
```

Search for:

```bash
server.py
```

Use:

```bash
find
```

Example:

```bash
find . -name "server.py"
```

### Production Connection

Finding configuration files:

```bash
find /etc -name "*.conf"
```

Finding logs:

```bash
find /var/log -name "*.log"
```

---

# Exercise 8 — Understand Home Directory

Answer:

```bash
echo $HOME
```

Navigate using:

```bash
cd ~
```

Questions:

1. Why is ~ useful?
2. Why do scripts use $HOME?

---

# Exercise 9 — Visualize Directory Trees

Install tree if needed.

Ubuntu:

```bash
sudo apt install tree
```

Display:

```bash
tree ~/linux-lab
```

Observe hierarchy.

---

# Exercise 10 — Filesystem Investigation

Investigate:

```bash
/
```

Visit:

```bash
/etc
/var
/tmp
/usr
/home
```

For each directory answer:

| Directory | Purpose |
| --------- | ------- |
| /etc      | ?       |
| /var      | ?       |
| /usr      | ?       |
| /tmp      | ?       |
| /home     | ?       |

---

# Real Production Scenario

A website is failing.

Logs indicate:

```bash
Configuration file not found
```

Expected:

```bash
/etc/nginx/nginx.conf
```

Actual:

```bash
/etc/nginx/conf/nginx.conf
```

Questions:

1. How would you locate the file?
2. Which commands help?
3. How would you verify the correct path?

Possible tools:

```bash
pwd
ls
find
tree
```

---

# Docker Connection

Containers are also filesystems.

Example:

```bash
docker exec -it container sh
```

Inside container:

```bash
/
├── app
├── etc
├── tmp
└── usr
```

The same navigation skills apply.

Understanding Linux paths directly improves Docker troubleshooting.

---

# Kubernetes Connection

Kubernetes mounts volumes into paths:

```yaml
volumeMounts:
  - mountPath: /data
```

If you do not understand filesystem hierarchy:

* Volume debugging becomes difficult.
* Persistent storage becomes confusing.

Filesystem knowledge scales directly into Kubernetes.

---

# Troubleshooting Challenges

## Challenge 1

You are here:

```text
/home/vip/projects/backend/api
```

Move to:

```text
/home/vip/documents
```

Using only relative paths.

---

## Challenge 2

Find every file ending in:

```text
.log
```

Inside:

```bash
~/linux-lab
```

---

## Challenge 3

Create:

```text
env/dev
env/staging
env/prod
```

Using a single command.

---

## Challenge 4

Return to previous directory without typing its full path.

Hint:

```bash
cd -
```

---

# Common Mistakes

## Mistake 1

Deleting from wrong directory.

```bash
rm *
```

Always verify:

```bash
pwd
```

first.

---

## Mistake 2

Confusing relative and absolute paths.

Bad:

```bash
cd documents
```

when directory doesn't exist.

Good:

```bash
cd ~/documents
```

---

## Mistake 3

Ignoring hidden files.

Many important files begin with:

```text
.
```

Examples:

```text
.bashrc
.gitconfig
.env
```

---

# Engineering Mindset

Beginners memorize commands.

Engineers understand location.

Ask:

```text
Where am I?
What filesystem am I operating in?
What path am I modifying?
What depends on this file?
```

This mindset prevents outages.

---

# Self-Assessment Checklist

Can you:

* Navigate without guessing?
* Explain absolute paths?
* Explain relative paths?
* Build directory structures quickly?
* Find files efficiently?
* Understand Linux hierarchy?
* Investigate unknown systems?

If yes, proceed to the next exercise.

---

# Interview Questions

### Beginner

1. What is the difference between absolute and relative paths?
2. What does pwd do?
3. What does cd .. mean?
4. What is the purpose of /home?

### Intermediate

5. Why are filesystem paths important in containers?
6. How would you locate a missing configuration file?
7. What is the difference between /tmp and /var?

### Advanced

8. Why does Kubernetes rely heavily on filesystem mounts?
9. How can incorrect filesystem assumptions cause production failures?
10. How does Linux namespace isolation affect filesystem visibility?

---

# Visual Summary

```mermaid
flowchart TD

A[Root /]
A --> B[/home]
A --> C[/etc]
A --> D[/var]
A --> E[/usr]
A --> F[/tmp]

B --> G[User Files]
C --> H[Configuration]
D --> I[Logs & Runtime Data]
E --> J[Programs]
F --> K[Temporary Data]
```

---

# Cheat Sheet

```bash
pwd                     # show current directory

ls                      # list files

ls -lah                 # detailed view

cd dir                  # enter directory

cd ..                   # parent directory

cd ~                    # home directory

cd -                    # previous directory

mkdir test              # create directory

mkdir -p a/b/c          # nested directories

touch file.txt          # create file

find . -name "*.txt"    # search files

tree                    # visualize hierarchy
```

---

# Completion Criteria

You successfully complete this exercise when you can:

1. Build filesystem structures from memory.
2. Navigate without getting lost.
3. Explain absolute vs relative paths.
4. Locate files quickly.
5. Understand how Linux organizes information.

Congratulations.

You now possess one of the most important foundational skills in Linux engineering.
