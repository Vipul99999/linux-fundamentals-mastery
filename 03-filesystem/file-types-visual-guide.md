# Linux File Types - Complete Visual Guide

> In Linux:
>
> ```text
> Everything Is A File
> ```
>
> But not all files are the same.
>
> Linux supports multiple file types that represent:
>
> * Regular files
> * Directories
> * Devices
> * Pipes
> * Sockets
> * Links
>
> Understanding file types is critical for:
>
> * Linux Administration
> * System Programming
> * Device Drivers
> * Networking
> * Containers
> * Kernel Development

---

# Big Picture

```text
Linux Files
│
├── Regular Files (-)
├── Directories (d)
├── Symbolic Links (l)
├── Character Devices (c)
├── Block Devices (b)
├── Named Pipes (p)
└── Sockets (s)
```

---

# How To View File Types

```bash
ls -l
```

Example:

```text
-rw-r--r-- file.txt
drwxr-xr-x Documents
lrwxrwxrwx shortcut -> file.txt
crw-rw-rw- tty0
brw-rw---- sda
prw-r--r-- mypipe
srwxrwxrwx mysocket
```

---

# Visual Legend

```text
- = Regular File
d = Directory
l = Symbolic Link
c = Character Device
b = Block Device
p = Named Pipe
s = Socket
```

---

# Complete Map

```text
Linux Kernel
│
├── Storage
│   └── Regular Files
│
├── Directories
│
├── Devices
│   ├── Character Devices
│   └── Block Devices
│
├── Process Communication
│   ├── Pipes
│   └── Sockets
│
└── Links
    └── Symbolic Links
```

---

# 1. Regular Files (-)

Most common file type.

Examples:

```text
notes.txt
app.py
main.c
photo.jpg
movie.mp4
```

---

# Visual

```text
File
 │
 ▼
Inode
 │
 ▼
Data Blocks
```

---

# Creation

```bash
touch file.txt
```

---

# Data Flow

```text
User
 │
 ▼
File
 │
 ▼
Disk Blocks
```

---

# Real Examples

```text
README.md
index.html
config.json
main.cpp
```

---

# 2. Directories (d)

Directories do NOT store file data.

They store:

```text
Filename
       │
       ▼
Inode Mapping
```

---

# Visual

```text
Directory
│
├── file1.txt
├── file2.txt
└── image.jpg
```

---

Internally:

```text
Directory
│
├── file1 -> inode 100
├── file2 -> inode 101
└── image -> inode 102
```

---

# Creation

```bash
mkdir projects
```

---

# Real Example

```text
/home
/etc
/usr
/var
```

---

# Directory Lookup Visual

```text
Filename
    │
    ▼
Directory Entry
    │
    ▼
Inode
    │
    ▼
Data
```

---

# 3. Symbolic Links (l)

Shortcut file.

---

Visual

```text
shortcut
    │
    ▼
target.txt
```

---

Creation

```bash
ln -s target.txt shortcut
```

---

# Internals

```text
Symlink
    │
    ▼
Path String
    │
    ▼
target.txt
```

---

# Real Example

```text
/bin -> /usr/bin
```

---

# Visual

```text
Application
      │
      ▼
Symlink
      │
      ▼
Real File
```

---

# 4. Character Devices (c)

Stream-oriented devices.

Data flows one byte at a time.

---

Examples

```text
/dev/tty
/dev/random
/dev/null
```

---

# Visual

```text
Application
      │
      ▼
Character Device
      │
      ▼
Driver
      │
      ▼
Hardware
```

---

# Example

```text
Keyboard
   │
   ▼
Character Stream
```

---

# Real Device Flow

```text
Keyboard
    │
    ▼
Driver
    │
    ▼
/dev/input/*
```

---

# 5. Block Devices (b)

Storage devices.

Data transferred in blocks.

---

Examples

```text
/dev/sda
/dev/nvme0n1
/dev/sdb
```

---

# Visual

```text
Application
      │
      ▼
Filesystem
      │
      ▼
Block Device
      │
      ▼
SSD/HDD
```

---

# Storage Stack

```text
File
 │
 ▼
Filesystem
 │
 ▼
Block Device
 │
 ▼
Disk
```

---

# Example

```text
/dev/sda
```

represents:

```text
Entire Disk
```

---

# 6. Named Pipes (p)

Used for process communication.

---

Creation

```bash
mkfifo mypipe
```

---

# Visual

```text
Process A
      │
      ▼
Named Pipe
      │
      ▼
Process B
```

---

# Data Flow

```text
Writer
   │
   ▼
Pipe
   │
   ▼
Reader
```

---

# Example

Terminal 1

```bash
echo hello > mypipe
```

Terminal 2

```bash
cat mypipe
```

---

# 7. Sockets (s)

Used for communication.

---

Examples

```text
MySQL
Docker
Nginx
Systemd
```

---

# Visual

```text
Client
   │
   ▼
Socket
   │
   ▼
Server
```

---

# Unix Socket Example

```text
/var/run/docker.sock
```

---

# Visual

```text
Docker CLI
      │
      ▼
docker.sock
      │
      ▼
Docker Daemon
```

---

# File Type Comparison

| Type      | Symbol | Stores Data | Use           |
| --------- | ------ | ----------- | ------------- |
| Regular   | -      | Yes         | Files         |
| Directory | d      | Names       | Organization  |
| Symlink   | l      | Path        | Shortcuts     |
| Character | c      | No          | Streams       |
| Block     | b      | No          | Storage       |
| Pipe      | p      | Temporary   | IPC           |
| Socket    | s      | Temporary   | Communication |

---

# How Linux Sees Them

```text
Everything
│
├── Storage
│   ├── Regular Files
│   └── Directories
│
├── Hardware
│   ├── Character Devices
│   └── Block Devices
│
├── Communication
│   ├── Pipes
│   └── Sockets
│
└── Navigation
    └── Symlinks
```

---

# Creation Commands

| Type         | Command         |
| ------------ | --------------- |
| Regular File | touch           |
| Directory    | mkdir           |
| Symlink      | ln -s           |
| Pipe         | mkfifo          |
| Device       | mknod           |
| Socket       | Program Creates |

---

# Real Linux System Example

```text
/
│
├── home
│   └── notes.txt
│
├── dev
│   ├── sda
│   ├── tty
│   └── random
│
├── var
│   └── docker.sock
│
└── tmp
    └── mypipe
```

---

# Memory Visualization

```text
Directory
     │
     ▼
Filename
     │
     ▼
Inode
     │
     ▼
File Type
     │
     ▼
Behavior
```

---

# Kernel Perspective

```text
VFS
│
├── Regular File Operations
├── Directory Operations
├── Device Operations
├── Pipe Operations
└── Socket Operations
```

---

# Interview Questions

## Beginner

1. What are Linux file types?
2. How many major file types exist?
3. What does d mean?
4. What does l mean?
5. What does b mean?

---

## Intermediate

6. Explain directory internals.
7. Explain block devices.
8. Explain character devices.
9. Explain pipes.
10. Explain sockets.

---

## Advanced

11. Explain VFS file abstractions.
12. Explain inode file type bits.
13. Explain device file registration.
14. Explain Unix domain sockets.
15. Explain pipe implementation.

---

# Final Mental Model

```text
Linux
│
├── Data
│   └── Regular Files
│
├── Organization
│   └── Directories
│
├── Navigation
│   └── Symlinks
│
├── Hardware
│   ├── Character Devices
│   └── Block Devices
│
└── Communication
    ├── Pipes
    └── Sockets
```

Remember:

```text
Not All Files Store Data

Some Files:
    Represent Hardware

Some Files:
    Represent Processes

Some Files:
    Represent Communication Channels

Some Files:
    Represent Other Files
```
