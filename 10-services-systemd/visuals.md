# Linux Services & systemd Visual Atlas

> A visual-first guide to understanding how Linux orchestrates an entire operating system.

---

# How To Use This File

Do NOT memorize visuals.

For every diagram ask:

```text
What is this?

↓

Why does it exist?

↓

What problem does it solve?

↓

What breaks if it fails?

↓

How does it connect to the whole system?
```

---

# Visual 1 : The Linux Layer Cake

This is the first visual every engineer should remember.

```mermaid
flowchart TD

Users["👨‍💻 Users"]

Applications["📦 Applications"]

Services["⚙️ Services"]

systemd["🧠 systemd"]

Kernel["🐧 Linux Kernel"]

Hardware["💻 Hardware"]

Users --> Applications

Applications --> Services

Services --> systemd

systemd --> Kernel

Kernel --> Hardware
```

Mental Model:

```text
Hardware

↓

Kernel

↓

systemd

↓

Services

↓

Applications

↓

Users
```

---

# Visual 2 : Linux Boot Process

```mermaid
flowchart TD

Power["⚡ Power On"]

Firmware["🖥️ BIOS / UEFI"]

Bootloader["🥾 GRUB"]

Kernel["🐧 Kernel"]

Initramfs["📦 Initramfs"]

systemd["🧠 systemd"]

Targets["🎯 Targets"]

Services["⚙️ Services"]

Users["👨‍💻 Users"]

Power --> Firmware

Firmware --> Bootloader

Bootloader --> Kernel

Kernel --> Initramfs

Initramfs --> systemd

systemd --> Targets

Targets --> Services

Services --> Users
```

---

# Visual 3 : systemd Architecture

```mermaid
flowchart TB

systemd["🧠 systemd"]

systemd --> Units["📦 Units"]

systemd --> DependencySolver["🕸️ Dependency Solver"]

systemd --> Logger["📝 Logging"]

systemd --> Scheduler["⏰ Timers"]

systemd --> ResourceManager["📊 Resource Manager"]

systemd --> Security["🛡️ Security"]

systemd --> ProcessManager["⚙️ Process Manager"]
```

---

# Visual 4 : Linux Is A Dependency Graph

This is one of the most important diagrams.

```mermaid
flowchart LR

Network["🌐 Network"]

PostgreSQL["🐘 PostgreSQL"]

Redis["🟥 Redis"]

API["⚙️ API"]

Nginx["🌍 Nginx"]

Users["👨‍💻 Users"]

Network --> PostgreSQL

Network --> Redis

PostgreSQL --> API

Redis --> API

API --> Nginx

Nginx --> Users
```

---

# Visual 5 : systemd Unit Ecosystem

```mermaid
mindmap

root((systemd Units))

Service

Target

Timer

Socket

Mount

Path

Swap

Device

Slice

Scope
```

---

# Visual 6 : Unit Relationships

```mermaid
flowchart TD

Units["📦 Units"]

Units --> Services["⚙️ Services"]

Units --> Targets["🎯 Targets"]

Units --> Timers["⏰ Timers"]

Units --> Mounts["💾 Mounts"]

Units --> Sockets["🔌 Sockets"]
```

---

# Visual 7 : Dependency Directives

```mermaid
flowchart TD

Dependencies

Dependencies --> Ordering

Dependencies --> Requirement

Ordering --> After

Ordering --> Before

Requirement --> Requires

Requirement --> Wants

Requirement --> BindsTo

Requirement --> PartOf

Requirement --> Requisite

Requirement --> Conflicts
```

---

# Visual 8 : Boot Targets

```mermaid
flowchart TD

basic["basic.target"]

network["network.target"]

multi["multi-user.target"]

graphical["graphical.target"]

rescue["rescue.target"]

basic --> network

network --> multi

multi --> graphical

multi --> rescue
```

---

# Visual 9 : Service Anatomy

```mermaid
flowchart TD

ServiceFile["📄 app.service"]

ServiceFile --> Unit["[Unit]"]

ServiceFile --> Service["[Service]"]

ServiceFile --> Install["[Install]"]

Unit --> Metadata["Metadata"]

Service --> Runtime["Runtime"]

Install --> Boot["Boot Integration"]
```

---

# Visual 10 : Service Lifecycle

```mermaid
stateDiagram-v2

[*] --> Inactive

Inactive --> Activating

Activating --> Active

Active --> Reloading

Reloading --> Active

Active --> Failed

Failed --> Restarting

Restarting --> Active

Active --> Stopping

Stopping --> Inactive
```

---

# Visual 11 : How systemctl Works

```mermaid
flowchart LR

Engineer["👨‍💻 Engineer"]

systemctl["⌨️ systemctl"]

systemd["🧠 systemd"]

Linux["🐧 Linux"]

Engineer --> systemctl

systemctl --> systemd

systemd --> Linux
```

---

# Visual 12 : Start vs Enable

```mermaid
flowchart LR

Start["▶️ Start"]

Now["Current Session"]

Enable["🔗 Enable"]

Boot["Future Boot"]

Start --> Now

Enable --> Boot
```

---

# Visual 13 : Timer Architecture

```mermaid
flowchart TD

Clock["🕐 Clock"]

Timer["⏰ Timer"]

systemd["🧠 systemd"]

Service["⚙️ Service"]

Logs["📝 Logs"]

Clock --> Timer

Timer --> systemd

systemd --> Service

Service --> Logs
```

---

# Visual 14 : Logging Architecture

```mermaid
flowchart TD

Kernel["🐧 Kernel"]

SSH["🔐 SSH"]

Docker["🐳 Docker"]

Nginx["🌍 Nginx"]

Redis["🟥 Redis"]

PostgreSQL["🐘 PostgreSQL"]

journald["📓 journald"]

journalctl["🔎 journalctl"]

Kernel --> journald

SSH --> journald

Docker --> journald

Nginx --> journald

Redis --> journald

PostgreSQL --> journald

journalctl --> journald
```

---

# Visual 15 : journald vs journalctl

```mermaid
flowchart LR

Events["📡 Events"]

journald["📓 journald"]

Storage["💾 Journal Database"]

journalctl["🔎 journalctl"]

Engineer["👨‍💻 Engineer"]

Events --> journald

journald --> Storage

Storage --> journalctl

journalctl --> Engineer
```

---

# Visual 16 : journald + rsyslog

```mermaid
flowchart TD

Applications["📦 Applications"]

journald["📓 journald"]

rsyslog["🚚 rsyslog"]

Local["💾 Local Files"]

Remote["☁️ Remote Logging"]

Applications --> journald

journald --> rsyslog

rsyslog --> Local

rsyslog --> Remote
```

---

# Visual 17 : Log Lifecycle

```mermaid
flowchart LR

Event

Event --> Log

Log --> Active

Active --> Rotated

Rotated --> Compressed

Compressed --> Archived

Archived --> Deleted
```

---

# Visual 18 : Troubleshooting Pyramid

```mermaid
flowchart TD

Configuration

Configuration --> Dependencies

Dependencies --> Resources

Resources --> Network

Network --> Application

Application --> Hardware
```

---

# Visual 19 : Production Investigation Workflow

```mermaid
flowchart TD

Alert

Alert --> systemctl

systemctl --> journalctl

journalctl --> Dependencies

Dependencies --> Resources

Resources --> Timeline

Timeline --> RootCause

RootCause --> Fix

Fix --> Prevention
```

---

# Visual 20 : Cascading Failures

This diagram is extremely important.

```mermaid
flowchart TD

DNS["🌐 DNS"]

Database["🐘 Database"]

API["⚙️ API"]

Nginx["🌍 Nginx"]

Users["👨‍💻 Users"]

DNS --> Database

Database --> API

API --> Nginx

Nginx --> Users

DNSX["❌ DNS Failure"]

DNSX --> DatabaseFailure

DatabaseFailure --> APIFailure

APIFailure --> WebsiteDown
```

---

# Visual 21 : Service Security Layers

```mermaid
flowchart TD

Application

Application --> UserIsolation

UserIsolation --> FilesystemIsolation

FilesystemIsolation --> Capabilities

Capabilities --> Namespaces

Namespaces --> cgroups

cgroups --> Kernel
```

---

# Visual 22 : Resource Management

```mermaid
flowchart TD

Service

Service --> CPU

Service --> Memory

Service --> Processes

Service --> IO

Service --> Network
```

---

# Visual 23 : Cloud Infrastructure Relationship

```mermaid
flowchart TD

systemd

systemd --> Docker

Docker --> containerd

containerd --> kubelet

kubelet --> Pods

Pods --> Applications
```

---

# Visual 24 : The Complete Production System

This is the most important visual in this folder.

```mermaid
flowchart TD

Users

Users --> Nginx

Nginx --> API

API --> Redis

API --> PostgreSQL

AllServices["⚙️ Services"]

Nginx --> AllServices

API --> AllServices

Redis --> AllServices

PostgreSQL --> AllServices

AllServices --> systemd

systemd --> journald

journald --> Engineers

Engineers --> ReliableSystems
```

---

# Visual 25 : The Entire Story

This is the final diagram.

```mermaid
flowchart TD

Hardware

Hardware --> Kernel

Kernel --> systemd

systemd --> DependencyGraph

DependencyGraph --> Services

Services --> Logs

Logs --> Engineers

Engineers --> Reliability

Reliability --> ProductionSystems
```

---

# The 5 Mental Models To Remember Forever

### 1

```text
Linux

≠

Programs

Linux

=

Coordinated System
```

---

### 2

```text
systemd

=

Operating System Orchestrator
```

---

### 3

```text
Services

=

Operating System Citizens
```

---

### 4

```text
Logs

=

Operating System Memory
```

---

### 5

```text
Troubleshooting

=

Reconstructing Reality
```

---

# The Ultimate One-Line Mental Model

```text
Hardware

↓

Kernel

↓

systemd

↓

Dependency Graph

↓

Services

↓

Logs

↓

Engineers

↓

Reliable Systems
```

That is the entire story of `10-services-systemd`.
