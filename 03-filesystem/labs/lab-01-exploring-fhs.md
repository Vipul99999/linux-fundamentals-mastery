# Lab 01 - Exploring Linux Directory Hierarchy

## Objective

Learn the Linux Filesystem Hierarchy Standard (FHS).

---

## Step 1

Display root directories.

```bash
ls /
```

Observe:

```text
bin
boot
dev
etc
home
proc
sys
usr
var
```

---

## Step 2

Explore configuration files.

```bash
ls /etc
```

Find:

```text
passwd
group
hosts
fstab
```

---

## Step 3

Explore user directories.

```bash
ls /home
```

---

## Step 4

Explore logs.

```bash
ls /var/log
```

---

## Step 5

Explore applications.

```bash
ls /usr/bin | head
```

---

## Questions

1. What is stored in /etc?
2. Why is /var different from /usr?
3. Why does Linux have a single root directory?
