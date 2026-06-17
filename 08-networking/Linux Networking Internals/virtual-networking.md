# Linux Virtual Networking

# How Linux Builds Entire Networks Without Physical Hardware

---

# Why This File Exists

Modern infrastructure is mostly virtual.

These systems all heavily depend on Linux virtual networking:

* Docker
* Kubernetes
* AWS
* Azure
* GCP
* KVM
* OpenStack
* Service Meshes

Most engineers think:

> Containers communicate using Docker.

That's incorrect.

The reality is:

> Docker, Kubernetes, and many cloud platforms are orchestration layers built on top of Linux virtual networking primitives.

Understanding this file is one of the biggest milestones in becoming a Linux, DevOps, SRE, or Cloud engineer.

---

# Learning Goal

By the end of this file, you should understand:

```text
Process

↓

Namespace

↓

veth

↓

Bridge

↓

NAT

↓

Host Interface

↓

Internet
```

Instead of:

```text
Docker magically works
```

---

# The Big Problem

Physical networking has limitations.

Imagine this machine.

```text
+----------------------+
|      Linux Host      |
|                      |
|        eth0          |
|   192.168.1.100      |
+----------------------+

          |

          |

      Internet
```

Suppose we create 50 containers.

Question:

```text
How can 50 containers communicate
using one physical NIC?
```

Linux solves this using virtual networking.

---

# Mental Model

Think of Linux as a city.

```text
Physical NIC = Highway

Bridge = Traffic Junction

veth = Roads

Namespaces = Buildings

Containers = Apartments

NAT = Translator
```

Linux constructs an entire city.

---

# The Building Blocks

Linux virtual networking consists of these components.

```text
Namespaces

↓

veth

↓

Bridge

↓

Routing

↓

NAT

↓

Physical Interface
```

Everything else is built from these.

---

# Virtual Networking Architecture

```text
+----------------------------------+
|          Container A             |
|           Namespace              |
|                                  |
|             eth0                 |
+----------------------------------+

                 |

              vethA

                 |

                 |

+----------------------------------+
|          Linux Bridge            |
|            docker0               |
+----------------------------------+

                 |

              vethB

                 |

+----------------------------------+
|          Container B             |
|           Namespace              |
|                                  |
|             eth0                 |
+----------------------------------+

                 |

                 |

+----------------------------------+
|          Host eth0               |
+----------------------------------+

                 |

             Internet
```

---

# Linux Virtual Networking Components

| Component    | Purpose                |
| ------------ | ---------------------- |
| Namespace    | Isolated network stack |
| veth         | Virtual cable          |
| Bridge       | Virtual switch         |
| Routing      | Path selection         |
| NAT          | Address translation    |
| Firewall     | Security               |
| Physical NIC | Real hardware          |

---

# Layer 1: Namespace

Namespace creates isolated network environments.

Each namespace owns:

```text
Interfaces

IP Addresses

Routes

Sockets

ARP Tables

Firewall Rules
```

Visualization:

```text
Host

├── Namespace A

├── Namespace B

└── Namespace C
```

---

# Layer 2: veth Pair

veth = Virtual Ethernet cable.

Imagine:

```text
veth0 <=================> veth1
```

Whatever enters one side exits the other side.

---

# Real Visualization

```text
Container Namespace

eth0

↓

veth0

<==============>

veth1

↓

Host Namespace
```

---

# Creating veth Pair

```bash
sudo ip link add veth0 type veth peer name veth1
```

Verify:

```bash
ip link
```

---

# Layer 3: Linux Bridge

A bridge behaves like a switch.

Think:

```text
Physical Switch

Port1

Port2

Port3
```

Linux version:

```text
Bridge

veth1

veth2

veth3
```

---

# Bridge Visualization

```text
           +----------------+

Container A|     Bridge      |Container B

vethA -----|    docker0      |----- vethB

           +----------------+

                   |

                Host eth0
```

---

# How Bridge Works

Bridge learns MAC addresses.

Example:

```text
MAC A → Port1

MAC B → Port2

MAC C → Port3
```

Packets are forwarded intelligently.

---

# Bridge Learning Process

```text
Packet arrives

↓

Read source MAC

↓

Store MAC

↓

Destination known?

↓

Yes → Forward

No → Broadcast
```

---

# Packet Journey Between Containers

Container A:

```text
10.0.0.2
```

Container B:

```text
10.0.0.3
```

Flow:

```text
Container A

↓

eth0

↓

vethA

↓

Bridge

↓

vethB

↓

eth0

↓

Container B
```

No physical NIC involved.

---

# Visualization

```text
+-----------+

Container A

10.0.0.2

+-----------+

      |

    vethA

      |

      |

+-----------+

Bridge

+-----------+

      |

    vethB

      |

+-----------+

Container B

10.0.0.3

+-----------+
```

---

# Host Communication

Containers can also communicate with the host.

```text
Container

↓

Bridge

↓

Host Namespace

↓

Application
```

---

# Internet Communication Problem

Question:

```text
How can 10.0.0.2 access internet?
```

The internet doesn't know:

```text
10.0.0.2
```

Linux uses NAT.

---

# NAT Visualization

```text
Container

10.0.0.2

↓

Bridge

↓

Host

192.168.1.100

↓

Internet
```

Linux changes:

```text
Source IP

10.0.0.2

↓

192.168.1.100
```

This is NAT.

---

# Packet Journey To Internet

```text
Container

↓

eth0

↓

veth

↓

Bridge

↓

Host

↓

iptables NAT

↓

eth0

↓

Router

↓

Internet
```

---

# Complete Virtual Networking Architecture

```text
                   INTERNET

                       |

                Home Router

                       |

                       |

                  +---------+

                  | eth0    |

                  | Host    |

                  +---------+

                       |

                 +------------+

                 | iptables   |

                 | NAT Engine |

                 +------------+

                       |

                +--------------+

                | docker0      |

                | Linux Bridge |

                +--------------+

                  /     |      \

                 /      |       \

              vethA   vethB   vethC

               |        |        |

         +----------+ +----------+ +----------+

         |ContainerA| |ContainerB| |ContainerC|

         |10.0.0.2  | |10.0.0.3  | |10.0.0.4  |

         +----------+ +----------+ +----------+
```

---

# Virtual Network Types

Linux supports multiple virtual network designs.

```text
Bridge

Host

Macvlan

Ipvlan

Overlay

VXLAN
```

We'll learn VXLAN later.

---

# Docker Network Drivers

Docker exposes Linux primitives.

| Docker Driver | Linux Primitive |
| ------------- | --------------- |
| bridge        | Linux bridge    |
| host          | Host namespace  |
| none          | No networking   |
| macvlan       | macvlan         |
| overlay       | VXLAN           |

Docker is mostly automation.

---

# Host Network Mode

Container shares host networking.

Visualization:

```text
Container

↓

Host Network Stack

↓

eth0

↓

Internet
```

Pros:

```text
Fast
```

Cons:

```text
No isolation
```

---

# Bridge Network Mode

Default Docker mode.

```text
Container

↓

veth

↓

Bridge

↓

Host

↓

Internet
```

Most common.

---

# None Mode

No networking.

```text
Container

↓

Nothing
```

Used for security.

---

# Macvlan

Container gets its own MAC address.

Visualization:

```text
Router

↓

Switch

↓

Container A

Container B

Container C
```

Looks like physical devices.

---

# Kubernetes Architecture

Pods also use virtual networking.

```text
Pod

↓

Namespace

↓

veth

↓

CNI

↓

Bridge

↓

Host
```

---

# Kubernetes Node Visualization

```text
+--------------------------------+

Node

+--------------------------------+

Pod A

↓

veth

↓

Bridge

↓

Pod B

↓

veth

↓

Bridge

↓

eth0

↓

Internet
```

---

# Cloud Networking Reality

Cloud is virtual networking at scale.

Example:

```text
VM

↓

Virtual NIC

↓

Virtual Switch

↓

Hypervisor

↓

Cloud Fabric

↓

Internet
```

---

# AWS Example

```text
EC2

↓

eth0

↓

ENA Driver

↓

Nitro Hypervisor

↓

AWS Fabric
```

Still Linux networking.

---

# Important Linux Kernel Components

Virtual networking heavily uses:

```text
Namespaces

sk_buff

Netfilter

Conntrack

Routing

Bridging

Traffic Control
```

---

# Production Troubleshooting

---

## Problem 1

Container cannot access internet.

Check:

```bash
ip route
```

---

## Problem 2

Containers cannot talk.

Check:

```bash
bridge link
```

---

## Problem 3

NAT issue.

Check:

```bash
sudo nft list ruleset
```

or

```bash
sudo iptables -t nat -L
```

---

## Problem 4

veth missing.

Check:

```bash
ip link
```

---

# Troubleshooting Decision Tree

```text
Container Broken

↓

Namespace Exists?

↓

veth Exists?

↓

Bridge Exists?

↓

IP Assigned?

↓

Route Exists?

↓

NAT Working?

↓

Host Internet Working?

↓

Firewall Blocking?
```

---

# Important Commands

Show interfaces:

```bash
ip link
```

Show bridges:

```bash
bridge link
```

Show bridge devices:

```bash
bridge vlan
```

Show routes:

```bash
ip route
```

Show neighbors:

```bash
ip neigh
```

Show NAT:

```bash
sudo nft list ruleset
```

Show sockets:

```bash
ss -tulnp
```

---

# Common Misconceptions

## Misconception 1

> Docker created virtual networking.

Wrong.

Linux already had it.

---

## Misconception 2

> Containers communicate directly.

Wrong.

They communicate through Linux networking components.

---

## Misconception 3

> Cloud networking is different.

Wrong.

Cloud scales Linux networking.

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

Bridge

↓

NAT

↓

Host eth0

↓

Router

↓

Internet
```

---

# Capability Checklist

After this file you should understand:

✅ Virtual networking

✅ veth

✅ Linux bridges

✅ NAT

✅ Container communication

✅ Internet access

✅ Docker networking

✅ Kubernetes networking

✅ Cloud networking foundations

---

# Repository Note

The next files become much easier because we now have the foundation.

```text
Completed:

✅ linux-network-stack.md

✅ kernel-networking.md

✅ network-namespaces.md

✅ virtual-networking.md

Upcoming:

bridge.md

vlan.md

vxlan.md

network-stack-visuals.md

packet-journey.md

network-troubleshooting.md
```

## Next File

```text
bridge.md
```

That file should go much deeper because Linux Bridge is one of the most important technologies behind:

* Docker
* Kubernetes
* KVM
* OpenStack
* Cloud networking
* Virtual machines
* Production Linux networking

And it deserves its own dedicated file.
