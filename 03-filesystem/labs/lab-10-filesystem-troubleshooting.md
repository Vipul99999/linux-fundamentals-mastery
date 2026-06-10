# Lab 10 - Filesystem Troubleshooting

## Objective

Learn common troubleshooting commands.

---

## Disk Usage

```bash
df -h
```

---

## Directory Usage

```bash
du -sh *
```

---

## Inode Usage

```bash
df -i
```

---

## Open Files

```bash
lsof | head
```

---

## Find Large Files

```bash
find /var -type f -size +100M
```

---

## Questions

1. Difference between df and du?
2. How do you identify inode exhaustion?
3. How do you find open files?
