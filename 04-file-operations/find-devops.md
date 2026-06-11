# find Command for DevOps Engineers

# Introduction

Most Linux tutorials teach:

```bash
find . -name "*.txt"
```

But DevOps engineers rarely search for text files.

In real production systems they search for:

```text
Dockerfiles
Kubernetes Manifests
Helm Charts
Environment Files
Logs
Certificates
Backups
Core Dumps
CI/CD Artifacts
Large Files
Configuration Files
```

This guide focuses on real-world DevOps usage.

---

# DevOps Mental Model

Think of a production system:

```text
Production Server
│
├── Application
├── Logs
├── Containers
├── Configurations
├── Certificates
├── Backups
└── Monitoring
```

When something breaks:

```text
You must locate files quickly.
```

That is where:

```bash
find
```

becomes essential.

---

# Visual Overview

```text
Production Server

/
│
├── etc
│
├── var
│
├── opt
│
├── home
│
└── srv
```

DevOps engineers constantly search these locations.

---

# Finding Dockerfiles

One of the most common tasks.

Search:

```bash
find . -name Dockerfile
```

Visual:

```text
Monorepo

services
│
├── auth
│   └── Dockerfile
│
├── payments
│   └── Dockerfile
│
└── users
    └── Dockerfile
```

Output:

```text
./auth/Dockerfile
./payments/Dockerfile
./users/Dockerfile
```

---

# Finding Docker Compose Files

```bash
find . -name "docker-compose*.yml"
```

Visual:

```text
project

docker-compose.yml
docker-compose.prod.yml
docker-compose.dev.yml
```

---

# Finding Container Logs

Search:

```bash
find /var/log -name "*.log"
```

Visual:

```text
/var/log

app.log
nginx.log
mysql.log
docker.log
```

Useful when:

```text
Application Failed
```

and you need logs.

---

# Finding Large Logs

Find logs larger than 500MB:

```bash
find /var/log -name "*.log" -size +500M
```

Visual:

```text
nginx.log      800MB ✓

app.log         50MB ✗
```

---

# Docker Volume Investigation

Docker volumes can consume huge disk space.

Find large files:

```bash
find /var/lib/docker -size +1G
```

Visual:

```text
Docker Storage

overlay2
│
├── layer1
├── layer2
└── huge-file.db
```

---

# Kubernetes Manifest Discovery

Find all Kubernetes YAML files:

```bash
find . -name "*.yaml"
```

Visual:

```text
k8s

deployment.yaml
service.yaml
ingress.yaml
```

---

# Search Specific Kubernetes Resources

Find Deployments:

```bash
find . -name "*deployment*.yaml"
```

Visual:

```text
frontend-deployment.yaml
backend-deployment.yaml
```

---

# Search Services

```bash
find . -name "*service*.yaml"
```

Visual:

```text
auth-service.yaml
payment-service.yaml
```

---

# Helm Chart Discovery

Find Helm charts:

```bash
find . -name Chart.yaml
```

Visual:

```text
helm

payment-chart
│
└── Chart.yaml

auth-chart
│
└── Chart.yaml
```

---

# Environment File Discovery

One of the most important DevOps tasks.

Find all environment files:

```bash
find . -name ".env*"
```

Visual:

```text
.env
.env.prod
.env.dev
.env.local
```

---

# Why This Matters

Environment files often contain:

```text
Database URLs
Secrets
API Keys
Tokens
```

DevOps engineers must know where they exist.

---

# Certificate Discovery

Find SSL certificates:

```bash
find /etc -name "*.crt"
```

Find private keys:

```bash
find /etc -name "*.key"
```

Visual:

```text
ssl

server.crt
server.key
```

---

# Warning

Private key searches often reveal:

```text
Security Risks
Expired Certificates
Unused Keys
```

---

# CI/CD Pipeline Usage

Modern pipelines use:

```bash
find
```

heavily.

Example:

Find all test files:

```bash
find . -name "*test*"
```

Visual:

```text
tests

auth.test.js
user.test.js
payment.test.js
```

---

# Locate Build Artifacts

Find generated builds:

```bash
find . -name "*.jar"
```

Java

```bash
find . -name "*.war"
```

Java EE

```bash
find . -name "*.apk"
```

Android

```bash
find . -name "*.ipa"
```

iOS

---

# Visual

```text
build

app.jar
backend.jar
```

---

# Searching Node.js Projects

Find package files:

```bash
find . -name package.json
```

Visual:

```text
service-a
│
└── package.json

service-b
│
└── package.json
```

---

# Finding node_modules

```bash
find . -name node_modules -type d
```

Useful when cleaning projects.

---

# Searching Python Projects

Find requirements files:

```bash
find . -name requirements.txt
```

Find virtual environments:

```bash
find . -name venv -type d
```

---

# Searching Go Projects

```bash
find . -name go.mod
```

Visual:

```text
go-service

go.mod
```

---

# Searching Rust Projects

```bash
find . -name Cargo.toml
```

---

# Searching Java Projects

```bash
find . -name pom.xml
```

Maven

```bash
find . -name build.gradle
```

Gradle

---

# Log Troubleshooting

Most production incidents involve logs.

Find all logs:

```bash
find /var/log -type f
```

Visual:

```text
app.log
auth.log
db.log
nginx.log
```

---

# Recent Logs

Modified in last hour:

```bash
find /var/log -mmin -60
```

Visual:

```text
Now
 │
 ▼
60 Minutes Ago
```

---

# Incident Response

Find files modified in last day:

```bash
find /etc -mtime -1
```

Visual:

```text
Yesterday
│
└── Config Changed ✓
```

Useful after outages.

---

# Disk Full Troubleshooting

One of the most common DevOps incidents.

Find files larger than 1GB:

```bash
find / -size +1G
```

Visual:

```text
backup.tar      5GB
database.sql   10GB
video.mp4       2GB
```

---

# Find Large Directories

Combined with du:

```bash
find /var -type d
```

Then inspect disk usage.

---

# Backup Discovery

Find backup archives:

```bash
find / -name "*.tar"
```

```bash
find / -name "*.tar.gz"
```

```bash
find / -name "*.zip"
```

Visual:

```text
backups

daily.tar.gz
weekly.tar.gz
monthly.tar.gz
```

---

# Core Dump Investigation

Application crashed?

Find core dumps:

```bash
find / -name "core*"
```

Visual:

```text
core.1234
core.5678
```

Useful for debugging.

---

# Monitoring Configuration Discovery

Find monitoring configs:

```bash
find /etc -name "*prometheus*"
```

```bash
find /etc -name "*grafana*"
```

Visual:

```text
prometheus.yml
grafana.ini
```

---

# Production Cleanup

Find temporary files:

```bash
find /tmp -type f
```

Find old temp files:

```bash
find /tmp -mtime +30
```

Visual:

```text
tmp

old-file.tmp ✓
```

---

# Monorepo Navigation

Large companies often have:

```text
Monorepo
│
├── frontend
├── backend
├── auth
├── payments
├── monitoring
└── infrastructure
```

Search for all Dockerfiles:

```bash
find . -name Dockerfile
```

Search for all Kubernetes manifests:

```bash
find . -name "*.yaml"
```

Search for all environment files:

```bash
find . -name ".env*"
```

---

# Performance Optimization

Bad:

```bash
find /
```

Visual:

```text
Entire System

10,000,000 Files
```

Good:

```bash
find /var/log
```

Visual:

```text
Specific Directory

10,000 Files
```

Always narrow search scope.

---

# DevOps Cheat Sheet

```text
Docker
│
└── find . -name Dockerfile

Kubernetes
│
└── find . -name "*.yaml"

Helm
│
└── find . -name Chart.yaml

Node.js
│
└── find . -name package.json

Python
│
└── find . -name requirements.txt

Go
│
└── find . -name go.mod

Rust
│
└── find . -name Cargo.toml

Java
│
└── find . -name pom.xml

Logs
│
└── find /var/log -type f

Large Files
│
└── find / -size +1G

Certificates
│
├── find /etc -name "*.crt"
│
└── find /etc -name "*.key"
```

---

# Visual Summary

```text
Production Problem
        │
        ▼
Need File
        │
        ▼
find
        │
        ▼
Locate Resource
        │
        ▼
Fix Problem
```

---

# Interview Questions

### Why is find important for DevOps?

Because production systems contain thousands to millions of files.

---

### How do you find Dockerfiles?

```bash
find . -name Dockerfile
```

---

### How do you find Kubernetes manifests?

```bash
find . -name "*.yaml"
```

---

### How do you find files larger than 1GB?

```bash
find / -size +1G
```

---

### How do you find recently modified configuration files?

```bash
find /etc -mtime -1
```

---

### Why should you avoid find /?

Because it searches the entire filesystem and can be slow.

---

# Key Takeaway

```text
Student:
find files

Administrator:
find system resources

DevOps Engineer:
find infrastructure components

SRE:
find root causes of incidents
```
