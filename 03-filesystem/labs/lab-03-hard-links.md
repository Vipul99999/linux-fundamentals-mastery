# Lab 03 - Hard Links

## Objective

Understand hard links.

---

## Step 1

Create file.

```bash
echo "Linux Lab" > file.txt
```

---

## Step 2

Create hard link.

```bash
ln file.txt backup.txt
```

---

## Step 3

Compare inode numbers.

```bash
ls -li
```

Observe:

```text
12345 file.txt
12345 backup.txt
```

---

## Step 4

Modify original.

```bash
echo "Updated" >> file.txt
```

Read backup.

```bash
cat backup.txt
```

---

## Step 5

Delete original.

```bash
rm file.txt
```

Read backup again.

```bash
cat backup.txt
```

---

## Questions

1. Why does backup still work?
2. What is link count?
3. Why isn't data deleted?
