# Linux Glossary

> This glossary contains important Linux terms, concepts, commands, and technologies that every Linux user, system administrator, DevOps engineer, and cloud engineer should understand.

---

# Table of Contents

* A
* B
* C
* D

---

# A

---

## Absolute Path

### Definition

A path that starts from the root directory (`/`) and specifies the complete location of a file or directory.

### Example

```bash
/home/vip/projects/app.js
```

### Visual

```text
/
└── home
    └── vip
        └── projects
            └── app.js
```

### Related Concepts

* Relative Path
* Filesystem
* Root Directory

---

## Alias

### Definition

A shortcut for a command.

### Example

```bash
alias ll='ls -la'
```

### Usage

Instead of:

```bash
ls -la
```

You can run:

```bash
ll
```

---

## API

### Definition

Application Programming Interface.

A set of rules that allows software components to communicate.

### Example

```text
Application
      ↓
API
      ↓
Linux Kernel
```

---

# B

---

## Background Process

### Definition

A process running without occupying the current terminal.

### Example

```bash
sleep 100 &
```

### Visual

```text
Terminal
    │
    ├── Foreground Process
    │
    └── Background Process
```

---

## Bash

### Definition

Bourne Again Shell.

The most widely used Linux shell.

### Example

```bash
echo "Hello Linux"
```

### Responsibilities

* Execute Commands
* Run Scripts
* Manage Variables
* Handle Pipes

---

## Binary File

### Definition

A file containing machine-readable data rather than plain text.

### Examples

```bash
/bin/ls
/bin/bash
```

---

## Bootloader

### Definition

Software that loads the operating system during startup.

### Boot Sequence

```text
Power On
    ↓
BIOS / UEFI
    ↓
Bootloader
    ↓
Kernel
```

### Examples

```text
GRUB
systemd-boot
```

---

# C

---

## Cgroup

### Definition

Control Groups (cgroups) limit and monitor resource usage.

### Controls

```text
CPU
Memory
Disk I/O
Network
```

### Used By

```text
Docker
Kubernetes
```

---

## CLI

### Definition

Command Line Interface.

A text-based interface for interacting with Linux.

### Example

```bash
ls
pwd
mkdir
```

---

## Cron

### Definition

A Linux scheduling service.

### Example

Run every day at midnight:

```bash
0 0 * * * /home/user/backup.sh
```

### Visual

```text
Cron
   ↓
Scheduled Task
   ↓
Automatic Execution
```

---

## Current Working Directory

### Definition

The directory in which the user is currently operating.

### Command

```bash
pwd
```

---

# D

---

## Daemon

### Definition

A background service running continuously.

### Examples

```text
sshd
nginx
docker
```

### Visual

```text
System Boot
      ↓
Daemon Starts
      ↓
Waits For Requests
```

---

## Device Driver

### Definition

Software that allows the kernel to communicate with hardware.

### Examples

```text
Network Driver
GPU Driver
Printer Driver
```

### Visual

```text
Application
      ↓
Kernel
      ↓
Driver
      ↓
Hardware
```

---

## Directory

### Definition

A container used to organize files.

### Example

```text
/home
/etc
/var
```

### Visual

```text
home/
├── user1
├── user2
└── projects
```

---

## Distribution (Distro)

### Definition

A complete operating system built around the Linux kernel.

### Formula

```text
Linux Kernel
     +
GNU Utilities
     +
Package Manager
     +
Applications
     =
Distribution
```

### Examples

```text
Ubuntu
Debian
Fedora
Arch Linux
Rocky Linux
```

---

## DNS

### Definition

Domain Name System.

Translates human-readable domain names into IP addresses.

### Example

```text
google.com
      ↓
142.250.x.x
```

### Visual

```text
User
  ↓

google.com
  ↓

DNS Server
  ↓

IP Address
```

---

## Docker

### Definition

A platform for creating and running containers.

### Visual

```text
Application
      ↓
Container
      ↓
Linux Kernel
      ↓
Hardware
```

### Related Concepts

* Containers
* cgroups
* Namespaces

---

# Quick Revision Table

| Term               | Short Meaning                    |
| ------------------ | -------------------------------- |
| Absolute Path      | Full file path                   |
| Alias              | Command shortcut                 |
| API                | Software communication interface |
| Background Process | Process running behind terminal  |
| Bash               | Linux shell                      |
| Binary File        | Machine-readable file            |
| Bootloader         | Loads operating system           |
| Cgroup             | Resource control mechanism       |
| CLI                | Command line interface           |
| Cron               | Task scheduler                   |
| Daemon             | Background service               |
| Device Driver      | Hardware communication software  |
| Directory          | Folder                           |
| Distribution       | Complete Linux OS                |
| DNS                | Name-to-IP translator            |
| Docker             | Container platform               |

---

# Next Sections

Future glossary sections:

```text
E
Environment Variable
Executable
Ext4

F
Filesystem
Fork
Foreground Process

G
GID
GNU
GRUB

H
Hard Link
Hostname

I
inode
Init
IPC

J
Job Control

K
Kernel
Kernel Space
Kubernetes

...

Z
Zombie Process
Zsh
```
