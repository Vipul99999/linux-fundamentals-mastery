# Linux Filesystem Visual Atlas

> This file provides visual maps of every major filesystem concept.

---

# Visual 1 — Complete Linux Storage Stack

```text
Application
      │
      ▼
System Call
(open/read/write)
      │
      ▼
VFS
(Virtual Filesystem)
      │
      ▼
Filesystem
(ext4/XFS/Btrfs)
      │
      ▼
Block Layer
      │
      ▼
Device Driver
      │
      ▼
SSD/HDD/NVMe
```

---

# Visual 2 — File Lookup

```text
Path
 │
 ▼
Directory
 │
 ▼
Dentry
 │
 ▼
Inode
 │
 ▼
Data Blocks
```

---

# Visual 3 — How a File is Stored

```text
file.txt
    │
    ▼
Directory Entry
    │
    ▼
Inode
    │
    ▼
Block 100
Block 101
Block 102
```

---

# Visual 4 — Linux Directory Tree

```text
/
│
├── bin
├── boot
├── dev
├── etc
├── home
├── proc
├── sys
├── tmp
├── usr
└── var
```

---

# Visual 5 — Mount Architecture

```text
/
│
├── home
│    │
│    ▼
│  /dev/sda2
│
├── boot
│    │
│    ▼
│  /dev/sda1
│
└── data
     │
     ▼
   /dev/sdb1
```

---

# Visual 6 — Hard Link

```text
file.txt
    │
    ▼
 inode 100
    ▲
    │
backup.txt
```

Same inode.

---

# Visual 7 — Symbolic Link

```text
shortcut
    │
    ▼
file.txt
    │
    ▼
inode 100
```

Different inode.

---

# Visual 8 — ext4 Architecture

```text
Filesystem
│
├── Superblock
├── Block Groups
├── Inode Tables
└── Data Blocks
```

---

# Visual 9 — XFS Architecture

```text
Filesystem
│
├── AG0
├── AG1
├── AG2
└── AG3
```

Allocation Groups.

---

# Visual 10 — Btrfs Architecture

```text
Filesystem
│
├── Subvolumes
├── Snapshots
├── Checksums
├── RAID
└── CoW
```

---

# Visual 11 — ProcFS

```text
Kernel
 │
 ▼
procfs
 │
 ▼
/proc
```

---

# Visual 12 — SysFS

```text
Hardware
     │
     ▼
Kernel Objects
     │
     ▼
/sys
```

---

# Visual 13 — Device Filesystem

```text
Hardware
     │
     ▼
Driver
     │
     ▼
/dev
```

---

# Visual 14 — tmpfs

```text
Application
      │
      ▼
tmpfs
      │
      ▼
RAM
```

---

# Visual 15 — Linux File Types

```text
Linux Files
│
├── Regular Files (-)
├── Directories (d)
├── Symlinks (l)
├── Character Devices (c)
├── Block Devices (b)
├── Pipes (p)
└── Sockets (s)
```

---

# Visual 16 — Character Device Flow

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

# Visual 17 — Block Device Flow

```text
Application
      │
      ▼
Filesystem
      │
      ▼
/dev/sda
      │
      ▼
Disk
```

---

# Visual 18 — Pipe Communication

```text
Process A
      │
      ▼
Pipe
      │
      ▼
Process B
```

---

# Visual 19 — Socket Communication

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

# Visual 20 — Complete Linux Filesystem Universe

```text
Linux Filesystem
│
├── Storage
│   ├── ext4
│   ├── XFS
│   └── Btrfs
│
├── Virtual Filesystems
│   ├── procfs
│   ├── sysfs
│   └── devtmpfs
│
├── Memory Filesystems
│   └── tmpfs
│
├── File Types
│   ├── Regular
│   ├── Directory
│   ├── Devices
│   ├── Pipes
│   ├── Sockets
│   └── Links
│
└── Storage Devices
    ├── HDD
    ├── SSD
    └── NVMe
```
