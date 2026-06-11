# tree.md

> **One picture is worth a thousand `ls` commands.**

The `tree` command is one of the most beginner-friendly and powerful Linux commands because it lets you **see the filesystem as a hierarchy**.

Without `tree`, directories can feel confusing.

With `tree`, the filesystem becomes visual.

---

# What is tree?

The `tree` command displays files and directories in a tree-like structure.

Instead of:

```bash
ls
```

showing:

```text
src
docs
README.md
package.json
```

`tree` shows:

```text
project
├── docs
├── package.json
├── README.md
└── src
```

Much easier to understand.

---

# Why tree Exists

Imagine a company organization.

Without structure:

```text
CEO
Manager1
Manager2
Engineer1
Engineer2
Engineer3
Intern1
Intern2
```

Confusing.

With structure:

```text
CEO
├── Manager1
│   ├── Engineer1
│   └── Engineer2
│
└── Manager2
    ├── Engineer3
    ├── Intern1
    └── Intern2
```

Immediately understandable.

Filesystems work the same way.

---

# Visual Comparison

## Using ls

```bash
ls
```

Output:

```text
backend
frontend
README.md
docker-compose.yml
```

You don't know what is inside.

---

## Using tree

```bash
tree
```

Output:

```text
project
├── backend
│   ├── controllers
│   ├── models
│   └── routes
│
├── frontend
│   ├── public
│   └── src
│
├── docker-compose.yml
└── README.md
```

Now you understand the entire structure.

---

# Installing tree

### Ubuntu/Debian

```bash
sudo apt install tree
```

### Fedora

```bash
sudo dnf install tree
```

### Arch Linux

```bash
sudo pacman -S tree
```

---

# Basic Usage

```bash
tree
```

Example:

```text
project
├── docs
├── README.md
├── src
└── tests
```

---

# Understanding Tree Symbols

Example:

```text
project
├── src
├── docs
└── tests
```

Meaning:

```text
project
│
├── src
│
├── docs
│
└── tests
```

---

# Symbol Explanation

| Symbol | Meaning              |
| ------ | -------------------- |
| `├──`  | Another item follows |
| `└──`  | Last item            |
| `│`    | Vertical connection  |
| `.`    | Current directory    |

---

# Visual Breakdown

```text
project
│
├── src
│
├── docs
│
└── tests
```

Hierarchy becomes visible.

---

# Real Project Example

```text
todo-app
├── backend
│   ├── controllers
│   ├── models
│   └── routes
│
├── frontend
│   ├── public
│   └── src
│
├── docker-compose.yml
└── README.md
```

This instantly communicates project architecture.

---

# Show Specific Directory

```bash
tree src
```

Output:

```text
src
├── components
├── pages
└── utils
```

---

# Show Hidden Files

Normally:

```bash
tree
```

doesn't show:

```text
.git
.env
```

Use:

```bash
tree -a
```

Output:

```text
project
├── .env
├── .git
├── README.md
└── src
```

---

# Visual

Without:

```text
README.md
src
```

With:

```text
.env
.git
README.md
src
```

---

# Limit Depth

Large projects can be overwhelming.

Example:

```bash
tree -L 2
```

Meaning:

```text
Show only 2 levels deep
```

---

# Visual

Full tree:

```text
project
├── src
│   ├── components
│   │   ├── Button
│   │   └── Modal
│   │
│   └── pages
│
└── tests
```

Limited:

```bash
tree -L 2
```

Output:

```text
project
├── src
│   ├── components
│   └── pages
│
└── tests
```

Cleaner.

---

# Show Directories Only

```bash
tree -d
```

Output:

```text
project
├── backend
├── frontend
├── docs
└── tests
```

Files hidden.

---

# Visual

Normal:

```text
backend
README.md
package.json
```

Directories only:

```text
backend
frontend
docs
```

---

# Show File Count

```bash
tree
```

Bottom:

```text
10 directories, 25 files
```

Useful for project analysis.

---

# Human-Friendly Sizes

```bash
tree -h
```

Output:

```text
README.md [2K]
logo.png [250K]
video.mp4 [25M]
```

---

# Show Permissions

```bash
tree -p
```

Output:

```text
[-rw-r--r--] README.md
[drwxr-xr-x] src
```

Useful for Linux learning.

---

# Show Ownership

```bash
tree -u
```

Output:

```text
README.md vip
src vip
```

---

# Show Group Ownership

```bash
tree -g
```

Output:

```text
README.md developers
src developers
```

---

# Combine Useful Options

```bash
tree -ah
```

Meaning:

```text
Show Hidden Files
Show Sizes
```

---

# Modern Development Use Cases

---

# Node.js Project

```text
project
├── package.json
├── package-lock.json
├── src
│   ├── controllers
│   ├── routes
│   └── services
│
└── tests
```

---

# React Project

```text
frontend
├── public
├── src
│   ├── components
│   ├── hooks
│   ├── pages
│   └── utils
│
└── package.json
```

---

# Python Project

```text
project
├── requirements.txt
├── app
├── tests
└── venv
```

---

# Docker Project

```text
project
├── Dockerfile
├── docker-compose.yml
├── backend
└── frontend
```

---

# Kubernetes Project

```text
k8s
├── deployment.yaml
├── ingress.yaml
└── service.yaml
```

---

# AI Project

```text
ai-project
├── datasets
├── models
├── notebooks
├── training
└── logs
```

---

# Documentation Generation

One major use of tree:

Generate project structure for README files.

Example:

```bash
tree -L 2
```

Copy output:

````markdown
```text
project
├── src
├── tests
└── docs
```
````

Used in GitHub repositories everywhere.

---

# Exclude Directories

Ignore node_modules:

```bash
tree -I "node_modules"
```

Visual:

Before:

```text
project
├── node_modules
│   ├── 5000 files
│   └── ...
└── src
```

After:

```text
project
└── src
```

Much cleaner.

---

# Exclude Multiple Patterns

```bash
tree -I "node_modules|dist|build"
```

---

# Save Output To File

```bash
tree > structure.txt
```

Output:

```text
structure.txt
```

contains project tree.

---

# Generate Documentation

```bash
tree -L 3 > project-structure.txt
```

Useful for:

```text
README files
Architecture docs
Technical documentation
```

---

# Security Use Cases

Audit hidden files:

```bash
tree -a
```

Review:

```text
.env
.git
.secret
.config
```

---

# DevOps Use Cases

View deployment structure:

```text
infra
├── docker
├── kubernetes
├── monitoring
└── scripts
```

Instant understanding.

---

# tree vs ls

| Feature             | ls      | tree      |
| ------------------- | ------- | --------- |
| Show Current Folder | Yes     | Yes       |
| Show Hierarchy      | No      | Yes       |
| Visual Structure    | No      | Yes       |
| Great For Projects  | Limited | Excellent |

---

# tree vs find

| Feature        | tree      | find      |
| -------------- | --------- | --------- |
| Visualization  | Excellent | Poor      |
| Searching      | Limited   | Excellent |
| Hierarchy View | Excellent | No        |
| File Discovery | Limited   | Excellent |

---

# Common Mistakes

## Printing Huge Trees

Bad:

```bash
tree /
```

Can produce enormous output.

Use:

```bash
tree -L 2 /
```

instead.

---

## Including node_modules

Bad:

```bash
tree
```

inside large Node.js projects.

Better:

```bash
tree -I "node_modules"
```

---

## Showing Too Much Depth

Bad:

```bash
tree
```

in giant monorepos.

Better:

```bash
tree -L 2
```

or

```bash
tree -L 3
```

---

# Visual Memory Map

```text
tree
│
├── Visualize Structure
│
├── Show Directories
│
├── Show Files
│
├── Show Hidden Files
│
├── Show Permissions
│
├── Show Sizes
│
└── Generate Documentation
```

---

# Hands-On Lab

Create:

```bash
mkdir -p project/src/components
mkdir -p project/docs
touch project/README.md
```

Run:

```bash
tree project
```

Output:

```text
project
├── docs
├── README.md
└── src
    └── components
```

---

# Interview Questions

### What does tree do?

Displays files and directories in a hierarchical tree structure.

---

### How do you show hidden files?

```bash
tree -a
```

---

### How do you show directories only?

```bash
tree -d
```

---

### How do you limit depth?

```bash
tree -L 2
```

---

### How do you exclude node_modules?

```bash
tree -I "node_modules"
```

---

### Why is tree useful?

It provides a visual representation of filesystem structure.

---

# Quick Cheat Sheet

```bash
tree

tree -a

tree -d

tree -L 2

tree -h

tree -p

tree -u

tree -g

tree -I "node_modules"

tree > structure.txt
```

---

# Key Takeaway

```text
ls
│
└── Shows Contents

find
│
└── Searches Contents

locate
│
└── Searches Database

tree
│
└── Visualizes Structure
```


which are used in almost every Linux command (`ls`, `cp`, `mv`, `rm`, `find`, scripts, automation, DevOps pipelines, etc.).
