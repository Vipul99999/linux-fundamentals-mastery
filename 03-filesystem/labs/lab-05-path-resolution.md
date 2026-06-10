# Lab 05 - Path Resolution

## Objective

Understand how Linux resolves paths.

---

## Step 1

Check current directory.

```bash
pwd
```

---

## Step 2

Create hierarchy.

```bash
mkdir -p lab/docs/reports
```

---

## Step 3

Navigate.

```bash
cd lab/docs/reports
```

---

## Step 4

Use relative path.

```bash
cd ..
```

---

## Step 5

Use parent path.

```bash
cd ../../
```

---

## Step 6

Resolve absolute path.

```bash
realpath docs
```

---

## Questions

1. Difference between relative and absolute paths?
2. What does "." mean?
3. What does ".." mean?
