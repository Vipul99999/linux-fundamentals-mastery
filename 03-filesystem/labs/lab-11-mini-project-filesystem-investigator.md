# Mini Project - Linux Filesystem Investigator

## Goal

Build a filesystem inspection report.

---

## Tasks

Collect:

### Filesystem Usage

```bash
df -h
```

### Inode Usage

```bash
df -i
```

### Mounts

```bash
findmnt
```

### CPU Information

```bash
cat /proc/cpuinfo
```

### Memory Information

```bash
cat /proc/meminfo
```

### Network Interfaces

```bash
ls /sys/class/net
```

---

## Generate Report

Save results:

```bash
report.txt
```

---

## Bonus

Write a Bash script:

```bash
filesystem-report.sh
```

that automatically generates:

```text
System Information
Filesystem Information
Memory Information
Network Information
```

into one report.
