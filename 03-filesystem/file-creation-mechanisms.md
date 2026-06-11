# File Creation Mechanisms in Linux

## Big Picture

A Linux file can be created by four different mechanisms:

```text
Linux File Creation
│
├── User-Created Files
├── Kernel-Created Files
├── Device-Created Files
└── Process-Created Files
```

---

# 1. User-Created Files

Created directly by users.

Examples:

```bash
touch notes.txt
echo hello > file.txt
nano app.py
vim config.conf
```

Visual:

```text
User
 │
 ▼
touch file.txt
 │
 ▼
Filesystem
 │
 ▼
Inode Created
 │
 ▼
File Exists
```

---

# 2. Kernel-Created Files

Created dynamically by kernel.

Examples:

```text
/proc/cpuinfo
/proc/meminfo
/sys/class/net
```

Visual:

```text
Kernel Data
     │
     ▼
Virtual Filesystem
     │
     ▼
File Appears
```

Important:

```text
File Exists
But No Disk Block Exists
```

---

# 3. Device-Created Files

Created when hardware appears.

Examples:

```text
/dev/sda
/dev/tty
/dev/random
```

Visual:

```text
USB Inserted
      │
      ▼
Kernel Detects
      │
      ▼
udev Event
      │
      ▼
/dev/sdb Created
```

---

# 4. Process-Created Files

Created temporarily by processes.

Examples:

```text
Sockets
Pipes
Shared Memory
Lock Files
```

Visual:

```text
Process
    │
    ▼
Creates Socket
    │
    ▼
Filesystem Entry
```

Example:

```text
/var/run/docker.sock
```

---

# Complete View

```text
Linux Files
│
├── Created By User
│
├── Created By Kernel
│
├── Created By Devices
│
└── Created By Processes
```
