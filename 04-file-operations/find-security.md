# find Command for Security, Auditing & Incident Response

# Introduction

Most Linux users think:

```bash
find . -name "*.txt"
```

is what `find` is for.

Security professionals think differently:

```text
What changed?

Who created this file?

Why is this executable here?

Why is this world-writable?

Why was this file modified yesterday?

Where are the private keys?

Which files violate compliance rules?
```

For security teams:

```bash
find
```

is one of the most important investigation tools on Linux.

---

# Security Mindset

Normal User:

```text
Find My Files
```

Security Engineer:

```text
Find Security Risks
```

Incident Responder:

```text
Find Evidence
```

Forensics Analyst:

```text
Find What Changed
```

---

# Security Investigation Workflow

```text
Security Alert
      │
      ▼
Gather Evidence
      │
      ▼
Locate Files
      │
      ▼
Analyze Metadata
      │
      ▼
Investigate Changes
      │
      ▼
Respond
```

`find` is used heavily during:

```text
Security Audits
Incident Response
Compliance Reviews
Forensics
Threat Hunting
```

---

# Visual Overview

```text
Linux Server

/
│
├── etc
├── var
├── home
├── opt
├── usr
└── tmp
```

Security engineers constantly search these areas.

---

# Finding World-Writable Files

One of the most common audit checks.

Search:

```bash
find / -perm -002
```

Meaning:

```text
Writable By Everyone
```

Visual:

```text
secure.txt

rw-r--r--
      ✗

public.txt

rw-rw-rw-
      ✓
```

---

# Why This Matters

World-writable files can lead to:

```text
Accidental Modification
Unauthorized Changes
Operational Risks
```

---

# Finding World-Writable Directories

```bash
find / -type d -perm -002
```

Visual:

```text
Directory

drwxrwxrwx
      ✓
```

Review whether such access is intended.

---

# Finding SUID Files

Linux supports special permissions.

Audit:

```bash
find / -perm -4000
```

Visual:

```text
Filesystem

sudo
passwd
su
```

These files deserve regular review.

---

# Understanding SUID

```text
Normal Program
      │
      ▼
Runs As User

SUID Program
      │
      ▼
Runs With File Owner Privileges
```

Because of their elevated behavior, organizations audit them carefully.

---

# Finding SGID Files

```bash
find / -perm -2000
```

Visual:

```text
Program
│
└── SGID Bit Set
```

Useful during compliance reviews.

---

# Finding Recently Modified Files

Investigations often start with:

```text
What changed recently?
```

Search:

```bash
find /etc -mtime -1
```

Meaning:

```text
Modified Within Last Day
```

Visual:

```text
Today
 │
 ▼
Yesterday

Changed ✓
```

---

# Incident Response Example

System stopped working.

Search:

```bash
find /etc -mtime -2
```

Investigate configuration changes before the incident.

---

# Find Recently Created Files

```bash
find /tmp -type f -mtime -1
```

Visual:

```text
tmp

new-file
   ✓
```

Useful for identifying newly added files.

---

# Search Recently Accessed Files

```bash
find /home -atime -1
```

Visual:

```text
Opened Recently
      │
      ▼
Found
```

Helpful for understanding activity.

---

# Finding Executable Files

Search:

```bash
find /tmp -type f -executable
```

Visual:

```text
tmp

run.sh       ✓
installer    ✓
notes.txt    ✗
```

Useful for audits and investigation.

---

# Searching Temporary Directories

Important locations:

```text
/tmp
/var/tmp
/dev/shm
```

Example:

```bash
find /tmp -type f
```

Visual:

```text
tmp

script.sh
data.txt
cache.tmp
```

Review unexpected files.

---

# Finding Hidden Files

Linux hidden files begin with:

```text
.
```

Search:

```bash
find /home -name ".*"
```

Visual:

```text
.bashrc
.profile
.hidden-file
```

Many are normal; some may warrant review.

---

# Search Hidden Directories

```bash
find /home -type d -name ".*"
```

Visual:

```text
.cache
.config
.local
```

---

# Finding Large Files

Security investigations often involve storage analysis.

Search:

```bash
find / -size +1G
```

Visual:

```text
backup.tar     5GB
video.iso      3GB
db.dump        8GB
```

Useful when:

```text
Disk Usage Suddenly Increased
```

---

# Finding Empty Files

```bash
find / -type f -empty
```

Visual:

```text
empty.log
     ✓
```

Useful during cleanup and auditing.

---

# Finding Empty Directories

```bash
find / -type d -empty
```

Visual:

```text
old-project/
     ✓
```

---

# Searching for Private Keys

Private keys are sensitive assets.

Search:

```bash
find /etc -name "*.key"
```

Visual:

```text
ssl

server.key
client.key
```

Review ownership and permissions.

---

# Finding Certificates

```bash
find /etc -name "*.crt"
```

Visual:

```text
server.crt
ca.crt
```

Useful during certificate audits.

---

# Finding Backup Files

Search:

```bash
find / -name "*.bak"
```

Visual:

```text
config.php.bak
database.bak
```

Old backups may contain sensitive data.

---

# Finding Configuration Files

```bash
find /etc -name "*.conf"
```

Visual:

```text
nginx.conf
sshd.conf
mysql.conf
```

Important during troubleshooting and reviews.

---

# Investigating Log Files

Search:

```bash
find /var/log -type f
```

Visual:

```text
auth.log
syslog
nginx.log
```

Logs are critical evidence sources.

---

# Finding Recently Changed Logs

```bash
find /var/log -mtime -1
```

Visual:

```text
auth.log
    ✓

nginx.log
    ✓
```

---

# Compliance Auditing

Organizations often check:

```text
Permissions
Ownership
Configuration Files
Certificates
Sensitive Data Locations
```

Using:

```bash
find
```

to collect information.

---

# Search By Owner

```bash
find / -user root
```

Visual:

```text
Owner

root
```

Useful during ownership reviews.

---

# Search By Group

```bash
find / -group wheel
```

or

```bash
find / -group sudo
```

depending on distribution.

---

# Search by Permission

Example:

```bash
find /etc -perm 777
```

Visual:

```text
777
│
└── Review Immediately
```

Highly permissive files should be reviewed.

---

# Forensics Timeline Investigation

Investigators often ask:

```text
What changed before the incident?
```

Example:

```bash
find / -mtime -7
```

Visual:

```text
Today
│
├── Day 1
├── Day 2
├── Day 3
├── Day 4
├── Day 5
├── Day 6
└── Day 7
```

Files inside this range become investigation candidates.

---

# Combining Conditions

Example:

```bash
find /tmp -type f -executable -mtime -1
```

Meaning:

```text
Files
AND
Executable
AND
Modified Recently
```

Visual:

```text
Candidate File
      │
      ▼
Executable?
      │
      ▼
Recent?
      │
      ▼
Result
```

---

# Search and Report

Display details:

```bash
find /var/log -name "*.log" -exec ls -lh {} \;
```

Visual:

```text
Find Log
    │
    ▼
Show Details
```

Useful during audits.

---

# Performance Considerations

Avoid:

```bash
find /
```

when possible.

Better:

```bash
find /etc
find /var/log
find /home
```

Visual:

```text
Entire Filesystem
│
└── Millions Of Files

Target Directory
│
└── Smaller Search Scope
```

---

# Common Security Investigation Locations

```text
/etc
│
└── Configuration

/var/log
│
└── Logs

/home
│
└── User Files

/tmp
│
└── Temporary Files

/var/tmp
│
└── Temporary Data

/opt
│
└── Applications
```

---

# Security Audit Checklist

```text
✓ World Writable Files

✓ World Writable Directories

✓ SUID Files

✓ SGID Files

✓ Recently Modified Configurations

✓ Large Unexpected Files

✓ Private Keys

✓ Certificates

✓ Backup Files

✓ Executable Files

✓ Log Files
```

---

# Visual Security Mind Map

```text
find
│
├── Permissions Audit
│
├── Ownership Audit
│
├── Configuration Audit
│
├── Certificate Audit
│
├── Log Investigation
│
├── Incident Response
│
├── Forensics
│
└── Compliance Review
```

---

# Interview Questions

### Why is find important in security?

Because it helps locate files relevant to audits, investigations, and incident response.

---

### How do you find world-writable files?

```bash
find / -perm -002
```

---

### How do you find SUID files?

```bash
find / -perm -4000
```

---

### How do you find recently modified configuration files?

```bash
find /etc -mtime -1
```

---

### How do you locate private keys?

```bash
find /etc -name "*.key"
```

---

### How do you search executable files?

```bash
find /tmp -type f -executable
```

---

# Key Takeaway

```text
Beginner:
Find Files

Administrator:
Find System Resources

Security Engineer:
Find Risks

Incident Responder:
Find Evidence

Forensics Analyst:
Find Timelines
```
