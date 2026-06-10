# Lab 08 - Virtual Filesystems

## Objective

Explore tmpfs and virtual filesystems.

---

## View Mounted Filesystems

```bash
mount
```

---

## View tmpfs

```bash
mount | grep tmpfs
```

---

## Filesystem Types

```bash
df -T
```

---

Observe:

```text
tmpfs
proc
sysfs
ext4
```

---

## Questions

1. Which filesystems are virtual?
2. Which are disk-based?
3. Why is tmpfs fast?
