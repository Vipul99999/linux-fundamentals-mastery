# Lab 04 - Symbolic Links

## Objective

Learn symbolic links.

---

## Step 1

Create file.

```bash
echo "Linux" > report.txt
```

---

## Step 2

Create symlink.

```bash
ln -s report.txt shortcut.txt
```

---

## Step 3

View.

```bash
ls -l
```

Observe:

```text
shortcut.txt -> report.txt
```

---

## Step 4

Read through symlink.

```bash
cat shortcut.txt
```

---

## Step 5

Delete target.

```bash
rm report.txt
```

---

## Step 6

Read symlink.

```bash
cat shortcut.txt
```

Observe:

```text
No such file
```

---

## Questions

1. Why did it fail?
2. How is symlink different from hard link?
3. What is a broken symlink?
