# System Calls

System Calls are the primary interface between user-space applications and the Linux kernel. They allow programs to request services from the operating system such as file operations, process creation, memory allocation, networking, device communication, and security-related operations.

Without system calls, applications cannot safely access hardware or kernel resources.

---

# Table of Contents

1. What are System Calls?
2. Why System Calls Exist
3. System Call Architecture
4. User Space vs Kernel Space
5. How a System Call Works
6. System Call Lifecycle
7. Categories of System Calls
8. Process Control System Calls
9. File Management System Calls
10. Device Management System Calls
11. Memory Management System Calls
12. Information Management System Calls
13. Communication System Calls
14. Security System Calls
15. System Call Flow Examples
16. System Call Table
17. Context Switching
18. System Call Overhead
19. Tracing System Calls
20. Real-World Examples
21. Advantages
22. Disadvantages
23. Interview Questions

---

# What are System Calls?

A System Call is a controlled entry point into the Linux kernel.

Applications cannot directly:

* Access hardware
* Modify kernel memory
* Control devices
* Manage processes

Instead they must use system calls.

---

# Why System Calls Exist

System calls provide:

* Security
* Resource Management
* Isolation
* Stability
* Hardware Abstraction

Without system calls:

```text
Application
      │
      ▼
Hardware
```

Every program could damage the system.

With system calls:

```text
Application
      │
      ▼
System Call
      │
      ▼
Kernel
      │
      ▼
Hardware
```

The kernel remains in control.

---

# System Call Architecture

```text
+--------------------------------+
|      User Application          |
+--------------------------------+
               │
               ▼
+--------------------------------+
|      C Library (glibc)         |
+--------------------------------+
               │
               ▼
+--------------------------------+
|      System Call Interface     |
+--------------------------------+
               │
               ▼
+--------------------------------+
|         Linux Kernel           |
+--------------------------------+
               │
               ▼
+--------------------------------+
|          Hardware              |
+--------------------------------+
```

---

# User Space vs Kernel Space

```text
+--------------------------------+
|           User Space           |
|--------------------------------|
| Chrome                         |
| Python                         |
| Node.js                        |
| Bash                           |
+--------------------------------+

          System Call

+--------------------------------+
|          Kernel Space          |
|--------------------------------|
| Scheduler                      |
| Memory Manager                 |
| Drivers                        |
| VFS                            |
| Networking                     |
+--------------------------------+
```

---

# How a System Call Works

Example:

```c
read(fd, buffer, 100);
```

Flow:

```text
Program
   │
   ▼
read()
   │
   ▼
System Call Trap
   │
   ▼
Kernel Mode
   │
   ▼
Kernel Service
   │
   ▼
Return Result
```

---

# System Call Lifecycle

```text
User Process
      │
      ▼
Invoke System Call
      │
      ▼
Switch to Kernel Mode
      │
      ▼
Kernel Validates Request
      │
      ▼
Kernel Executes Operation
      │
      ▼
Result Generated
      │
      ▼
Return to User Mode
```

---

# Mode Switching

System calls require switching CPU privilege levels.

```text
User Mode
     │
     ▼
System Call
     │
     ▼
Kernel Mode
     │
     ▼
Execute Service
     │
     ▼
User Mode
```

This is called a **Context Switch**.

---

# Categories of System Calls

```text
System Calls
│
├── Process Control
├── File Management
├── Device Management
├── Memory Management
├── Information Management
├── Communication
└── Security
```

---

# Process Control System Calls

Used for creating and managing processes.

Common examples:

| System Call | Purpose           |
| ----------- | ----------------- |
| fork()      | Create process    |
| exec()      | Execute program   |
| wait()      | Wait for child    |
| exit()      | Terminate process |
| kill()      | Send signal       |

---

## fork()

Creates a child process.

```c
pid_t pid = fork();
```

Visualization:

```text
Parent Process
        │
        ▼
      fork()
        │
        ▼
 ┌──────────────┐
 │              │
 ▼              ▼
Parent       Child
```

---

## exec()

Replaces current process image.

```c
execvp("ls", args);
```

Flow:

```text
Process
   │
   ▼
exec()
   │
   ▼
Old Program Removed
   │
   ▼
New Program Loaded
```

---

## wait()

Waits for child completion.

```c
wait(NULL);
```

```text
Parent
  │
  ▼
wait()
  │
  ▼
Child Finishes
  │
  ▼
Parent Continues
```

---

# File Management System Calls

Used for file operations.

Common calls:

| System Call | Purpose      |
| ----------- | ------------ |
| open()      | Open file    |
| read()      | Read file    |
| write()     | Write file   |
| close()     | Close file   |
| lseek()     | Move pointer |

---

## File Access Flow

```text
Application
      │
      ▼
open()
      │
      ▼
Kernel
      │
      ▼
VFS
      │
      ▼
Filesystem Driver
      │
      ▼
Storage Device
```

---

## open()

```c
int fd = open("data.txt", O_RDONLY);
```

Returns:

```text
File Descriptor
```

Example:

```text
0 → stdin
1 → stdout
2 → stderr
3 → data.txt
```

---

## read()

```c
read(fd, buffer, size);
```

Visualization:

```text
Disk
 │
 ▼
Kernel Buffer
 │
 ▼
Application Buffer
```

---

## write()

```c
write(fd, msg, length);
```

Visualization:

```text
Application
     │
     ▼
Kernel Buffer
     │
     ▼
Disk
```

---

# Device Management System Calls

Used for hardware interaction.

Examples:

```c
ioctl()
read()
write()
```

Flow:

```text
Application
      │
      ▼
System Call
      │
      ▼
Device Driver
      │
      ▼
Hardware
```

---

# Memory Management System Calls

Memory-related operations.

Examples:

| Call       | Purpose            |
| ---------- | ------------------ |
| brk()      | Heap Growth        |
| mmap()     | Map Memory         |
| munmap()   | Unmap Memory       |
| mprotect() | Change Permissions |

---

## mmap()

Maps files into memory.

```c
mmap(...)
```

Visualization:

```text
File
 │
 ▼
Memory Mapping
 │
 ▼
Virtual Address Space
```

Benefits:

* Faster access
* Reduced copying
* Shared memory

---

# Information Management System Calls

Retrieve system information.

Examples:

```c
getpid()
getuid()
uname()
time()
```

---

## getpid()

```c
pid_t pid = getpid();
```

Returns:

```text
Current Process ID
```

---

# Communication System Calls

Used for IPC (Inter Process Communication).

Examples:

```c
pipe()
socket()
msgget()
shmget()
```

---

## Pipe Communication

```text
Process A
     │
     ▼
   pipe()
     │
     ▼
Process B
```

Example:

```bash
ls | grep txt
```

---

## Socket Communication

```text
Client
   │
   ▼
Socket
   │
   ▼
Network
   │
   ▼
Socket
   │
   ▼
Server
```

Common Calls:

```c
socket()
bind()
listen()
accept()
connect()
send()
recv()
```

---

# Security System Calls

Control permissions and identity.

Examples:

```c
chmod()
chown()
setuid()
setgid()
access()
```

---

## Permission Flow

```text
User
 │
 ▼
System Call
 │
 ▼
Kernel Security Check
 │
 ▼
Allow / Deny
```

---

# System Call Table

Every system call has a unique number.

Example:

```text
0   read
1   write
2   open
3   close
39  getpid
57  fork
59  execve
60  exit
```

Kernel uses these IDs internally.

---

# Context Switching

When a system call occurs:

```text
User Mode
     │
     ▼
Save Registers
     │
     ▼
Kernel Mode
     │
     ▼
Execute Request
     │
     ▼
Restore Registers
     │
     ▼
User Mode
```

---

# System Call Overhead

System calls are slower than normal function calls.

Reason:

```text
Mode Switch
Permission Checks
Memory Validation
Context Save/Restore
```

Example:

```text
Function Call
     │
     ▼
Nanoseconds

System Call
     │
     ▼
Hundreds of Nanoseconds
or Microseconds
```

---

# Tracing System Calls

Linux provides tools.

---

## strace

Monitor system calls.

Example:

```bash
strace ls
```

Output:

```text
open()
read()
write()
close()
```

---

## ltrace

Trace library calls.

```bash
ltrace ls
```

---

# Real-World Example 1

Command:

```bash
cat notes.txt
```

Flow:

```text
cat
 │
 ▼
open()
 │
 ▼
read()
 │
 ▼
write()
 │
 ▼
close()
```

---

# Real-World Example 2

Running a Program

```bash
./app
```

Flow:

```text
Shell
 │
 ▼
fork()
 │
 ▼
Child Process
 │
 ▼
execve()
 │
 ▼
Program Loaded
```

---

# Real-World Example 3

Opening a Website in Chrome

```text
Chrome
  │
  ▼
socket()
  │
  ▼
connect()
  │
  ▼
send()
  │
  ▼
recv()
  │
  ▼
Display Page
```

---

# Advantages

### Security

Kernel validates requests.

### Stability

Applications cannot directly access hardware.

### Hardware Independence

Same API across devices.

### Resource Management

Kernel controls resource usage.

### Process Isolation

Processes remain independent.

---

# Disadvantages

### Performance Cost

Mode switching overhead.

### Kernel Complexity

Large implementation.

### Debugging Difficulty

Harder than user-space debugging.

### Limited Direct Control

Applications must go through kernel.

---

# Most Important System Calls

```text
fork()
execve()
wait()
open()
read()
write()
close()
mmap()
socket()
connect()
accept()
pipe()
kill()
getpid()
exit()
```

Every Linux application ultimately relies on these calls.

---

# Interview Questions

## Beginner

1. What is a system call?
2. Why are system calls required?
3. Difference between user mode and kernel mode?
4. What is open()?
5. What is read()?

## Intermediate

6. Explain fork().
7. Explain exec().
8. What is mmap()?
9. What is a file descriptor?
10. What is a context switch?

## Advanced

11. How does a system call reach the kernel?
12. Explain system call overhead.
13. What happens internally during fork()?
14. Explain execve().
15. How does Linux implement sockets?

---

# Summary

System Calls form the boundary between user applications and the Linux kernel. They provide controlled access to operating system services such as process management, file operations, memory management, networking, device control, and security. Every Linux application—from simple commands like `ls` to large applications like Chrome, Docker, and databases—relies on system calls to communicate with the kernel safely and efficiently.
