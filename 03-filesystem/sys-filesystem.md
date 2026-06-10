# Sys Filesystem (sysfs) in Linux

> If `/proc` provides information about **processes and kernel state**,
>
> then `/sys` provides information about **hardware devices, drivers, buses, kernel objects, and system topology**.
>
> The Sys Filesystem (sysfs) is one of the most important virtual filesystems in Linux and forms the foundation for:
>
> - Device Management
> - Hardware Discovery
> - udev
> - systemd
> - Containers
> - Embedded Linux
> - Driver Development

---

# Table of Contents

1. What is SysFS?
2. Why SysFS Exists
3. SysFS Architecture
4. SysFS and the Linux Device Model
5. How SysFS Works
6. Mounting SysFS
7. SysFS Directory Structure
8. Devices in SysFS
9. Device Classes
10. Buses
11. Block Devices
12. Network Devices
13. Kernel Objects (kobjects)
14. Drivers
15. Modules
16. Power Management
17. Writing to SysFS
18. udev and SysFS
19. SysFS vs ProcFS
20. Real World Examples
21. Security Considerations
22. Troubleshooting
23. Interview Questions

---

# What is SysFS?

SysFS is a virtual filesystem that exposes:

```text
Kernel Objects
Hardware Devices
Device Drivers
Buses
Kernel Subsystems
```

as files and directories.

---

## Mount Point

```text
/sys
```

---

## Important

Files in `/sys`:

```text
Do Not Exist On Disk
```

They are generated dynamically by the kernel.

---

# Visual

```text
Hardware Device
       │
       ▼
Kernel Object
       │
       ▼
sysfs
       │
       ▼
/sys Files
```

---

# Why SysFS Exists

Before SysFS:

```text
Device Information
Driver Information
Hardware Relationships
```

were scattered across the kernel.

---

SysFS provides:

```text
Unified Hardware View
```

---

# Linux Philosophy

Instead of:

```c
get_device_information()
```

Linux allows:

```bash
cat /sys/class/net/eth0/address
```

---

# Visual

```text
Application
      │
      ▼
Read File
      │
      ▼
Kernel Object
      │
      ▼
Hardware Information
```

---

# SysFS Architecture

```text
Hardware
      │
      ▼
Device Driver
      │
      ▼
Kernel Objects
      │
      ▼
sysfs
      │
      ▼
/sys
```

---

# Linux Device Model

SysFS is built on the Linux Device Model.

---

# Components

```text
Devices
Drivers
Buses
Classes
```

---

# Visual

```text
Linux Device Model
│
├── Devices
├── Drivers
├── Classes
└── Buses
```

---

# How SysFS Works

Suppose:

```bash
cat /sys/class/net/eth0/address
```

Linux performs:

```text
Read File
      │
      ▼
Kernel Driver
      │
      ▼
Network Device
      │
      ▼
Return MAC Address
```

---

# Mounting SysFS

Check:

```bash
mount | grep sysfs
```

---

Example:

```text
sysfs on /sys type sysfs
```

---

## Manual Mount

```bash
mount -t sysfs sysfs /sys
```

---

# Top-Level Structure

```text
/sys
│
├── block
├── bus
├── class
├── devices
├── firmware
├── fs
├── kernel
├── module
└── power
```

---

# Visual Overview

```text
/sys
│
├── Hardware
├── Drivers
├── Buses
├── Devices
├── Kernel
└── Power
```

---

# /sys/devices

Contains the actual hardware hierarchy.

---

# Visual

```text
/sys/devices
│
├── pci0000:00
├── cpu
├── platform
└── virtual
```

---

# Hardware Tree

```text
Motherboard
      │
      ▼
PCI Bus
      │
      ▼
Network Card
      │
      ▼
Storage Controller
```

represented inside:

```text
/sys/devices
```

---

# Example

```bash
ls /sys/devices
```

---

# /sys/class

Provides logical device classes.

---

# Visual

```text
/sys/class
│
├── net
├── block
├── tty
├── power_supply
└── thermal
```

---

# Purpose

Groups similar devices together.

---

Example:

```text
All Network Interfaces
```

under:

```text
/sys/class/net
```

---

# Network Devices

```bash
ls /sys/class/net
```

Output:

```text
eth0
wlan0
lo
```

---

# Visual

```text
Network Devices
│
├── eth0
├── wlan0
└── lo
```

---

# View MAC Address

```bash
cat /sys/class/net/eth0/address
```

---

Example:

```text
00:11:22:33:44:55
```

---

# View Interface State

```bash
cat /sys/class/net/eth0/operstate
```

Output:

```text
up
```

or

```text
down
```

---

# Block Devices

Directory:

```text
/sys/block
```

---

Contains:

```text
sda
sdb
nvme0n1
loop0
```

---

# Visual

```text
/sys/block
│
├── sda
├── sdb
├── nvme0n1
└── loop0
```

---

# Disk Information

Example:

```bash
cat /sys/block/sda/size
```

---

Output:

```text
Number of sectors
```

---

# Device Relationship

```text
Disk
 │
 ▼
Kernel Device
 │
 ▼
sysfs Entry
```

---

# /sys/bus

Contains hardware buses.

---

Examples:

```text
PCI
USB
I2C
SCSI
```

---

# Visual

```text
/sys/bus
│
├── pci
├── usb
├── i2c
└── scsi
```

---

# PCI Example

```bash
ls /sys/bus/pci/devices
```

---

Shows:

```text
PCI Devices
```

---

# USB Example

```bash
ls /sys/bus/usb/devices
```

---

Shows:

```text
USB Devices
```

---

# /sys/module

Contains loaded kernel modules.

---

Visual

```text
/sys/module
│
├── ext4
├── usbcore
├── snd
└── xfs
```

---

# Example

```bash
ls /sys/module
```

---

# Module Information

```bash
ls /sys/module/ext4
```

---

Shows:

```text
Parameters
References
Settings
```

---

# /sys/kernel

Contains kernel subsystem information.

---

Visual

```text
/sys/kernel
│
├── debug
├── tracing
├── security
└── config
```

---

# Important Subsystems

---

## tracefs

```text
/sys/kernel/tracing
```

Kernel tracing.

---

## debugfs

```text
/sys/kernel/debug
```

Kernel debugging.

---

## securityfs

```text
/sys/kernel/security
```

Security modules.

---

# /sys/power

Power management controls.

---

Visual

```text
/sys/power
│
├── state
├── wakeup_count
└── mem_sleep
```

---

# Example

```bash
cat /sys/power/state
```

---

Output:

```text
freeze
mem
disk
```

---

# Kernel Objects (kobjects)

The heart of sysfs.

---

Every device becomes:

```text
Kernel Object
```

called:

```text
kobject
```

---

# Visual

```text
Hardware
    │
    ▼
kobject
    │
    ▼
sysfs Entry
```

---

# Example

```text
Network Card
      │
      ▼
kobject
      │
      ▼
/sys/class/net/eth0
```

---

# Writing to SysFS

Unlike many ProcFS files:

SysFS often allows writing.

---

Example:

```bash
echo performance > \
/sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
```

---

Visual

```text
User
 │
 ▼
Write File
 │
 ▼
Kernel Object Updated
 │
 ▼
Hardware Behavior Changes
```

---

# CPU Information

Directory:

```text
/sys/devices/system/cpu
```

---

Visual

```text
CPU
│
├── cpu0
├── cpu1
├── cpu2
└── cpu3
```

---

# Example

```bash
cat /sys/devices/system/cpu/online
```

---

Output:

```text
0-7
```

---

# Memory Information

Directory:

```text
/sys/devices/system/memory
```

---

Shows:

```text
Memory Blocks
Hotplug Information
```

---

# udev and SysFS

When hardware is connected:

```text
USB Inserted
      │
      ▼
Kernel Detects
      │
      ▼
sysfs Updated
      │
      ▼
udev Reads sysfs
      │
      ▼
Creates /dev Entry
```

---

# Visual

```text
USB Device
      │
      ▼
sysfs
      │
      ▼
udev
      │
      ▼
/dev/sdb
```

---

# SysFS vs ProcFS

| Feature | SysFS | ProcFS |
|----------|----------|----------|
| Mount Point | /sys | /proc |
| Focus | Hardware | Processes |
| Device Model | Yes | No |
| Kernel Parameters | Some | Many |
| Process Data | No | Yes |
| Hardware Data | Yes | Limited |
| Driver Data | Yes | Limited |

---

# Visual Comparison

```text
/proc
 │
 ├── Processes
 ├── Memory
 ├── CPU
 └── Kernel State


/sys
 │
 ├── Devices
 ├── Drivers
 ├── Buses
 └── Hardware
```

---

# Real World Examples

---

# Find Network MAC Address

```bash
cat /sys/class/net/eth0/address
```

---

# Check SSD Information

```bash
ls /sys/block
```

---

# View CPU Online Cores

```bash
cat /sys/devices/system/cpu/online
```

---

# View USB Devices

```bash
ls /sys/bus/usb/devices
```

---

# Kubernetes

Uses:

```text
sysfs
procfs
cgroups
```

extensively.

---

# Docker

Reads:

```text
/sys
```

for hardware and resource information.

---

# Embedded Linux

Heavy sysfs usage for:

```text
GPIO
Sensors
Power Management
```

---

# Security Considerations

Some sysfs files allow:

```text
System Configuration Changes
```

---

Examples:

```text
CPU Governors
Power Settings
Driver Parameters
```

---

Require proper permissions.

---

# Common Misconceptions

❌ SysFS stores hardware

✔ SysFS exposes hardware information

---

❌ SysFS files are normal files

✔ Generated dynamically by kernel

---

❌ SysFS duplicates ProcFS

✔ Different purposes

---

❌ SysFS only shows devices

✔ Also exposes drivers, buses, modules, and kernel objects

---

# Troubleshooting Commands

---

## Show SysFS Mount

```bash
mount | grep sysfs
```

---

## Show Network Devices

```bash
ls /sys/class/net
```

---

## Show Block Devices

```bash
ls /sys/block
```

---

## Show PCI Devices

```bash
ls /sys/bus/pci/devices
```

---

## Show USB Devices

```bash
ls /sys/bus/usb/devices
```

---

## Show Loaded Modules

```bash
ls /sys/module
```

---

# Complete SysFS Map

```text
/sys
│
├── devices
│
├── class
│
├── block
│
├── bus
│
├── module
│
├── kernel
│
├── power
│
├── firmware
│
└── fs
```

---

# Interview Questions

## Beginner

1. What is SysFS?
2. Why does Linux use SysFS?
3. What is mounted at `/sys`?
4. What is `/sys/class/net`?
5. What is `/sys/block`?
6. What is `/sys/module`?
7. What is a kernel object?
8. What is a device class?
9. What is a bus?
10. How is SysFS different from ProcFS?

---

## Intermediate

11. Explain SysFS architecture.
12. Explain Linux Device Model.
13. Explain kobjects.
14. Explain device classes.
15. Explain sysfs file generation.
16. Explain block device representation.
17. Explain network interface representation.
18. Explain module exposure.
19. Explain bus representation.
20. Explain udev interaction.

---

## Advanced

21. Explain sysfs implementation.
22. Explain kobject lifecycle.
23. Explain sysfs attribute files.
24. Explain driver model integration.
25. Explain uevents.
26. Explain device hotplug handling.
27. Explain sysfs permissions.
28. Explain sysfs performance characteristics.
29. Explain container interaction with sysfs.
30. Explain sysfs internals within VFS.

---

# Summary

```text
Hardware
    │
    ▼
Kernel Device Model
    │
    ▼
kobjects
    │
    ▼
sysfs
    │
    ▼
/sys Files
```

Core Idea:

```text
SysFS
      =
Hardware
      +
Devices
      +
Drivers
      +
Buses
      +
Kernel Objects
```

Remember:

```text
/proc
      =
Processes & Runtime State

/sys
      =
Hardware & Device Model
```

Understanding SysFS is essential before learning:

- Device Drivers
- Linux Kernel Development
- udev
- Containers
- cgroups
- Embedded Linux
- Hardware Monitoring
- Performance Tuning
- Linux Device Management
