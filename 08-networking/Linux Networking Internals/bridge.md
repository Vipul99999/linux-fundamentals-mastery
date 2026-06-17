# Linux Bridge

# Understanding Linux Virtual Switches

---

# Why this file exists

Linux Bridge is one of the most important technologies in modern infrastructure.

It powers:

* Docker
* Kubernetes
* KVM
* OpenStack
* LXC
* Podman
* Cloud platforms

If you understand Linux Bridge deeply, Docker networking becomes easy.

---

# Big Picture

A Linux bridge is simply:

> A software switch inside Linux.

Think:

```text
Physical World

Laptop A ----┐

Laptop B ----┼---- Physical Switch ---- Router

Laptop C ----┘
```

Linux does the same thing virtually.

```text
Virtual World

Container A ----┐

Container B ----┼---- Linux Bridge ---- Host NIC

Container C ----┘
```

---

# Visual 1: Linux Bridge Mental Model

```text
┌──────────────────────────────────────────────┐
│              Linux Host                      │
│                                              │
│                                              │
│  Container A                                │
│      │                                       │
│    vethA                                     │
│      │                                       │
│      │                                       │
│  Container B                                │
│      │                                       │
│    vethB                                     │
│      │                                       │
│      │                                       │
│  Container C                                │
│      │                                       │
│    vethC                                     │
│      │                                       │
│      ▼                                       │
│                                              │
│   ┌───────────────────────┐                  │
│   │     Linux Bridge      │                  │
│   │       docker0         │                  │
│   └───────────────────────┘                  │
│                │                             │
│                │                             │
│             Host eth0                        │
│                │                             │
└────────────────┼─────────────────────────────┘
                 │
                 ▼
            Internet
```

---

# The Problem Linux Bridge Solves

Imagine 100 containers.

Question:

```text
How do all containers communicate?

Without 100 physical NICs?
```

Linux Bridge solves this.

---

# Without Bridge

Impossible.

```text
Container A

↓

Internet


Container B

↓

Internet


Container C

↓

Internet
```

No local network.

No switch.

No communication.

---

# With Bridge

```text
               ┌───────────────┐
               │ Linux Bridge  │
               └──────┬────────┘

          ┌───────────┼───────────┐

          │           │           │

       vethA       vethB       vethC

          │           │           │

          │           │           │

    ContainerA  ContainerB  ContainerC
```

Now all containers are in one network.

---

# Mental Model

Treat Linux Bridge exactly like a switch.

```text
Switch Terminology

Port

↓

Cable

↓

MAC Table

↓

Forward Packet
```

Linux Bridge has the same things.

| Physical Switch | Linux Bridge |
| --------------- | ------------ |
| Port            | veth         |
| MAC Table       | FDB          |
| Cable           | veth         |
| Switch          | bridge       |
| Device          | Namespace    |

---

# Visual 2: Physical vs Linux

```text
PHYSICAL NETWORK

Laptop A

     │

Laptop B

     │

Laptop C

     │

┌─────────────┐

│   Switch    │

└─────────────┘

     │

 Router

---------------------------------

LINUX NETWORK

Container A

     │

Container B

     │

Container C

     │

┌─────────────┐

│ LinuxBridge │

└─────────────┘

     │

 Host NIC
```

---

# Bridge Components

Linux Bridge has 4 major components.

```text
┌───────────────────────┐

        Linux Bridge

└───────────────────────┘

          │

 ┌────────┼────────┐

 │        │        │

 ▼        ▼        ▼

Ports    FDB     STP
```

---

# Component 1: Ports

Bridge ports are attached interfaces.

Examples:

```text
veth

tap

eth0

vxlan
```

Think:

```text
Container

↓

veth

↓

Bridge Port
```

---

# Component 2: FDB

FDB = Forwarding Database.

Extremely important.

Stores:

```text
MAC Address

↓

Bridge Port
```

---

# Visual 3: FDB Table

```text
┌─────────────────────────────────┐

      Forwarding Database

┌─────────────────────────────────┐

MAC Address          Port

AA:11:22:33:44:55 → vethA

BB:11:22:33:44:55 → vethB

CC:11:22:33:44:55 → vethC

└─────────────────────────────────┘
```

---

# How Learning Works

Suppose:

Container A sends packet.

```text
Packet arrives

↓

Bridge reads source MAC

↓

Store source MAC

↓

Destination MAC known?

↓

YES → Forward

NO → Broadcast
```

This is identical to a physical switch.

---

# Visual 4: Learning Process

```text
             Packet Arrives

                    │

                    ▼

     ┌─────────────────────────┐

     │ Read Source MAC Address │

     └─────────────────────────┘

                    │

                    ▼

     ┌─────────────────────────┐

     │ Store in FDB           │

     └─────────────────────────┘

                    │

                    ▼

     ┌─────────────────────────┐

     │ Destination Known ?     │

     └─────────────────────────┘

           │             │

          YES            NO

           │             │

           ▼             ▼

      Forward        Broadcast
```

---

# Visual 5: Packet Journey

Container A talks to Container B.

```text
┌────────────┐

│Container A │

│ 10.0.0.2   │

└─────┬──────┘

      │

    eth0

      │

    vethA

      │

      ▼

┌─────────────────┐

│ Linux Bridge    │

│ docker0         │

└────────┬────────┘

         │

       vethB

         │

       eth0

         │

         ▼

┌────────────┐

│Container B │

│ 10.0.0.3   │

└────────────┘
```

No physical NIC involved.

---

# Broadcast Domain

Everything attached to a bridge belongs to one broadcast domain.

Example:

```text
ARP Request

Who has 10.0.0.3?
```

Bridge broadcasts.

```text
Container A

        │

        ▼

Linux Bridge

  │   │   │

  ▼   ▼   ▼

 B   C   D
```

---

# Docker Architecture

Docker automatically builds this.

```text
┌─────────────────────────────┐

Host

┌──────────────────────────┐

docker0 Bridge

└──────────────────────────┘

  │         │          │

vethA     vethB      vethC

  │         │          │

  ▼         ▼          ▼

ContainerA ContainerB ContainerC

└─────────────────────────────┘
```

---

# Kubernetes Architecture

Kubernetes uses bridges too.

```text
Pod

↓

veth

↓

Bridge

↓

Host Network

↓

Internet
```

CNI plugins automate this.

---

# Production Architecture

```text
                 INTERNET

                     │

             Home Router

                     │

                  Host NIC

                     │

      ┌─────────────────────────┐

      │ Linux Bridge (docker0)  │

      └─────────────────────────┘

          │       │       │

       vethA   vethB   vethC

          │       │       │

          ▼       ▼       ▼

      Auth    Payment   Redis
```

---

# Create A Bridge

```bash
sudo ip link add br0 type bridge
```

Verify:

```bash
ip link
```

Bring it up.

```bash
sudo ip link set br0 up
```

---

# Attach Interface

```bash
sudo ip link set veth1 master br0
```

Verify:

```bash
bridge link
```

---

# View FDB

```bash
bridge fdb show
```

---

# Troubleshooting Visual

```text
Container Cannot Reach Another Container

                 │

                 ▼

         Bridge Exists ?

           │        │

          NO       YES

           │        │

      Create It     ▼

             veth Attached ?

               │         │

              NO        YES

               │         │

          Attach It      ▼

             IP Exists ?

               │         │

              NO        YES

               │         │

           Add IP        ▼

             Firewall ?

               │         │

              YES       NO

               │         │

            Fix Rule   Success
```

---

# Production Problems

### Problem 1

Bridge doesn't exist.

```bash
ip link
```

---

### Problem 2

veth detached.

```bash
bridge link
```

---

### Problem 3

Wrong subnet.

```bash
ip addr
```

---

### Problem 4

Firewall blocking.

```bash
sudo nft list ruleset
```

---

# Essential Commands

Create bridge

```bash
ip link add br0 type bridge
```

Delete bridge

```bash
ip link delete br0
```

View bridge

```bash
bridge link
```

View FDB

```bash
bridge fdb show
```

View interfaces

```bash
ip link
```

---

# Engineer Mental Model

Never think:

```text
Container

↓

Internet
```

Always think:

```text
Container

↓

Namespace

↓

veth

↓

Linux Bridge

↓

NAT

↓

Host NIC

↓

Router

↓

Internet
```

---

# Capability Checklist

After this file you should understand:

✅ Linux Bridge

✅ Virtual switch concept

✅ FDB

✅ Broadcast domains

✅ Container communication

✅ Docker bridge networking

✅ Kubernetes bridge networking

✅ Production troubleshooting


ository look genuinely world-class.
