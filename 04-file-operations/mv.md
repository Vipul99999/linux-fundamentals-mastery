# mv Command (Move and Rename Files & Directories)

# What is mv?

`mv` stands for:

```text
Move
```

The `mv` command is used to:

```text
1. Move files
2. Move directories
3. Rename files
4. Rename directories
```

Unlike `cp`, it does not create a duplicate.

It changes the location or name of the original item.

---

# Why Do We Need mv?

Imagine you have a notebook.

Current location:

```text
Bedroom
│
└── Math Notebook
```

You decide to place it in another room.

```text
Study Room
```

Result:

```text
Bedroom
│
└── (empty)

Study Room
│
└── Math Notebook
```

The notebook moved.

It was not copied.

Linux works exactly the same way.

---

# Real-Life Analogy

Think of moving a chair.

Before:

```text
Living Room
│
└── Chair
```

Move chair to bedroom.

After:

```text
Living Room
│
└── (empty)

Bedroom
│
└── Chair
```

Only one chair exists.

Linux equivalent:

```bash
mv chair.txt bedroom/
```

---

# Relationship with Previous Commands

```text
touch
│
└── Create File

cp
│
└── Copy File

mv
│
└── Move Or Rename File

rm
│
└── Delete File
```

---

# Syntax

```bash
mv source destination
```

Example:

```bash
mv notes.txt Documents/
```

---

# Understanding Move

Suppose:

```text
Current Directory
│
├── notes.txt
│
└── backup
```

Command:

```bash
mv notes.txt backup/
```

---

# Visual Before Move

```text
Current Directory
│
├── notes.txt
│
└── backup
```

---

# Visual After Move

```text
Current Directory
│
└── backup
    │
    └── notes.txt
```

Notice:

```text
notes.txt
```

no longer exists in the original location.

---

# Copy vs Move

This is one of the most important Linux concepts.

---

## cp

Command:

```bash
cp notes.txt backup.txt
```

Result:

```text
notes.txt
backup.txt
```

Two files exist.

---

Visual:

```text
Original
     │
     ▼
Duplicate
```

---

## mv

Command:

```bash
mv notes.txt backup/
```

Result:

```text
backup/notes.txt
```

Only one file exists.

---

Visual:

```text
Old Location
      │
      ▼
New Location
```

---

# Internal Working

When Linux performs:

```bash
mv notes.txt backup/
```

Linux updates filesystem entries.

```text
Old Directory
      │
      ▼
Remove Entry
      │
      ▼
Create Entry
      │
      ▼
New Directory
```

Usually faster than copying.

---

# Why mv Is Fast

Copying:

```text
Read Data
    │
    ▼
Duplicate Data
    │
    ▼
Write Data
```

Moving inside the same filesystem:

```text
Update Location Information
```

No data duplication required.

---

# Your First Move

Create:

```bash
touch notes.txt

mkdir archive
```

Current:

```text
.
├── notes.txt
└── archive
```

Move:

```bash
mv notes.txt archive/
```

Result:

```text
.
└── archive
    └── notes.txt
```

---

# Moving Multiple Files

Create:

```bash
touch a.txt b.txt c.txt

mkdir backups
```

Move:

```bash
mv a.txt b.txt c.txt backups/
```

Result:

```text
backups
│
├── a.txt
├── b.txt
└── c.txt
```

---

# Visual Example

Before:

```text
.
├── a.txt
├── b.txt
├── c.txt
└── backups
```

After:

```text
.
└── backups
    │
    ├── a.txt
    ├── b.txt
    └── c.txt
```

---

# Renaming Files

Most beginners don't realize this.

Suppose:

```text
notes.txt
```

Command:

```bash
mv notes.txt homework.txt
```

Result:

```text
homework.txt
```

No copy created.

Name changed.

---

# Visual Rename

Before:

```text
notes.txt
```

Command:

```bash
mv notes.txt homework.txt
```

After:

```text
homework.txt
```

Think of changing a person's name.

The person stays the same.

Only the name changes.

---

# Internal Rename Visualization

```text
File
│
├── Data
├── Permissions
├── Owner
└── Metadata
```

After rename:

```text
File
│
├── Data
├── Permissions
├── Owner
└── Metadata
```

Only:

```text
Name
```

changes.

---

# Renaming Directories

Before:

```text
project
│
├── app.js
└── index.html
```

Command:

```bash
mv project website
```

After:

```text
website
│
├── app.js
└── index.html
```

Everything remains intact.

---

# Visual Rename Directory

Before:

```text
project
```

After:

```text
website
```

Contents unchanged.

---

# Move and Rename Together

Suppose:

```text
Downloads
│
└── report.txt
```

Command:

```bash
mv report.txt archive/final_report.txt
```

Result:

```text
archive
│
└── final_report.txt
```

Moved and renamed simultaneously.

---

# Visual

Before:

```text
report.txt
```

After:

```text
archive
│
└── final_report.txt
```

---

# Interactive Mode

Option:

```bash
mv -i file destination
```

If destination exists:

```text
Overwrite?
```

Linux asks before replacing.

---

Visual:

```text
Existing File
      │
      ▼
New File
      │
      ▼
Ask User
```

---

# Force Mode

```bash
mv -f file destination
```

Linux overwrites immediately.

---

Visual:

```text
Old File
    │
    ▼
Removed

New File
    │
    ▼
Written
```

---

# Verbose Mode

```bash
mv -v old.txt new.txt
```

Output:

```text
renamed 'old.txt' -> 'new.txt'
```

Useful for learning.

---

# Common Errors

## File Doesn't Exist

Command:

```bash
mv abc.txt backup/
```

Output:

```text
No such file or directory
```

---

Visual:

```text
Looking For:

abc.txt

Found?

No
```

---

## Destination Doesn't Exist

Command:

```bash
mv file.txt unknown/
```

Output:

```text
No such file or directory
```

Because:

```text
unknown
```

directory doesn't exist.

---

## Permission Denied

Command:

```bash
mv notes.txt /root/
```

Output:

```text
Permission denied
```

---

# Real-World Use Cases

## Organizing Downloads

Before:

```text
Downloads
│
├── photo.jpg
├── report.pdf
└── movie.mp4
```

Move:

```bash
mv *.jpg Pictures/
```

---

Result:

```text
Pictures
│
└── photo.jpg
```

---

## Renaming Files

```bash
mv draft.txt final.txt
```

---

## Deployment

```bash
mv app.jar /opt/application/
```

---

## Archiving Logs

```bash
mv server.log logs/
```

---

# Hands-On Practice Lab

Create:

```bash
mkdir practice

cd practice

touch notes.txt
```

Check:

```bash
ls
```

Output:

```text
notes.txt
```

---

Rename:

```bash
mv notes.txt homework.txt
```

Check:

```bash
ls
```

Output:

```text
homework.txt
```

---

Create:

```bash
mkdir archive
```

Move:

```bash
mv homework.txt archive/
```

Check:

```bash
tree
```

Expected:

```text
.
└── archive
    └── homework.txt
```

---

Create:

```bash
touch a.txt b.txt c.txt

mkdir backups
```

Move:

```bash
mv a.txt b.txt c.txt backups/
```

Expected:

```text
backups
│
├── a.txt
├── b.txt
└── c.txt
```

---

# Memory Trick

Think:

```text
cp
│
└── Photocopy Machine

mv
│
└── Moving Truck
```

Photocopy:

```text
Original
+
Copy
```

Moving Truck:

```text
Old Place
      │
      ▼
New Place
```

Only one item exists.

---

# Visual Summary

```text
touch
│
└── Create

cp
│
└── Duplicate

mv
│
├── Move
│
└── Rename

rm
│
└── Delete
```

---

# Interview Questions

## What does mv stand for?

```text
Move
```

---

## Can mv rename files?

Yes.

Example:

```bash
mv old.txt new.txt
```

---

## Difference Between cp and mv?

```text
cp
│
└── Creates Duplicate

mv
│
└── Moves Original
```

---

## Can mv move directories?

Yes.

Example:

```bash
mv project backup/
```

---

## What does mv -i do?

Interactive mode.

Asks before overwrite.

---

## Why is mv usually faster than cp?

Because moving within the same filesystem often only updates directory entries rather than copying file data.

---

# Quick Summary

```text
Command:
mv

Purpose:
Move Files
Move Directories
Rename Files
Rename Directories

Examples:

mv notes.txt archive/

mv old.txt new.txt

mv project website

Remember:

touch → Create

cp → Copy

mv → Move/Rename

rm → Delete
```

---

# Learning Flow

```text
pwd
│
├── Where am I?

ls
│
├── What is here?

cd
│
├── Go somewhere

mkdir
│
├── Create place

touch
│
├── Create file

cp
│
├── Duplicate file

mv
│
└── Move/Rename file
```
