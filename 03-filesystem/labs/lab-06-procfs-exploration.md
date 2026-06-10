# Lab 06 - Exploring ProcFS

## Objective

Learn ProcFS.

---

## CPU Information

```bash
cat /proc/cpuinfo
```

---

## Memory Information

```bash
cat /proc/meminfo
```

---

## Uptime

```bash
cat /proc/uptime
```

---

## Current Process

```bash
echo $$
```

Example:

```text
1234
```

---

Explore:

```bash
ls /proc/1234
```

---

## Open Files

```bash
ls -l /proc/$$/fd
```

---

## Questions

1. Are proc files stored on disk?
2. What information does procfs expose?
3. How does top use procfs?
