# Lab 02 - Understanding Inodes

## Objective

Learn how Linux stores files using inodes.

---

## Step 1

Create a file.

```bash
touch report.txt
```

---

## Step 2

View inode.

```bash
ls -i report.txt
```

Example:

```text
45231 report.txt
```

---

## Step 3

View detailed metadata.

```bash
stat report.txt
```

Observe:

- inode number
- permissions
- timestamps
- links

---

## Step 4

Write data.

```bash
echo "Linux" > report.txt
```

Check inode again.

```bash
ls -i report.txt
```

---

## Questions

1. Did inode change?
2. What metadata is stored in inode?
3. Is filename stored in inode?
