# Subnetting Visuals

> Build visual intuition for subnetting using diagrams, flows, mental models, and engineering perspectives.

---

# Why This File Exists

Subnetting is difficult because people try to memorize formulas.

Wrong approach:

```text
/24 = 256

/26 = 64

/27 = 32
```

Memorization eventually fails.

Instead build intuition.

---

# Master Mental Model

Subnetting is simply:

```text
One Large Network

↓

Split

↓

Smaller Networks

↓

Organize

↓

Secure

↓

Scale
```

---

# Visual 1: Giant Network Problem

Without subnetting:

```text
Company Network

192.168.1.0/24

┌─────────────────────────────┐

Laptop

Phone

TV

Printer

Camera

Server

HR

Engineering

Finance

Security

└─────────────────────────────┘
```

Problems:

```text
Too many broadcasts

↓

Congestion

↓

Difficult management
```

---

# Visual 2: Subnetting Solution

```text
Company

↓

Engineering

192.168.1.0/26

↓

HR

192.168.1.64/27

↓

Finance

192.168.1.96/27

↓

Security

192.168.1.128/27
```

Everything becomes organized.

---

# Visual 3: House Analogy

Without organization:

```text
1 Giant Building

↓

10,000 People
```

Chaos.

---

With organization:

```text
Building

↓

Floor

↓

Room

↓

Person
```

Networking:

```text
Internet

↓

Network

↓

Subnet

↓

Device
```

---

# Visual 4: Network + Host

Example:

```text
192.168.1.25/24
```

Visualization:

```text
192.168.1 | 25

Network | Host
```

---

# Visual 5: Binary Boundary

```text
192.168.1.25/24

11000000

10101000

00000001

00011001


11111111

11111111

11111111

00000000
```

Boundary:

```text
NETWORK | HOST
```

---

# Visual 6: The Scissors Analogy

Think:

```text
/24

↓

Scissors

↓

Split

↓

Smaller Networks
```

Example:

```text
192.168.1.0/24

↓

192.168.1.0/25

192.168.1.128/25
```

---

# Visual 7: How Splitting Works

```text
192.168.1.0/24

256 addresses

↓

Split

↓

192.168.1.0/25

128

↓

192.168.1.128/25

128
```

---

# Visual 8: Keep Splitting

```text
/24

256

↓

/25

128

↓

/26

64

↓

/27

32

↓

/28

16

↓

/29

8

↓

/30

4
```

---

# Visual 9: Bigger Prefix = Smaller Network

Many beginners get this wrong.

Visual:

```text
/8

█████████████████████

Huge


/16

█████████████

Large


/24

██████

Medium


/28

██

Small
```

Rule:

```text
Bigger Prefix

↓

Smaller Network
```

---

# Visual 10: Host Space Shrinks

```text
/24

Network

████████████████████████

Host

████████


/25

Network

█████████████████████████

Host

███████


/26

Network

██████████████████████████

Host

██████
```

Rule:

```text
More Network Bits

↓

Less Host Space
```

---

# Visual 11: Broadcast Domain Reduction

Without subnetting:

```text
1000 Devices

↓

Broadcast

↓

1000 Devices Receive
```

---

With subnetting:

```text
250

↓

Broadcast

↓

250 Devices Receive
```

Much better.

---

# Visual 12: Home Network

```text
Internet

↓

Router

192.168.1.1

↓

Laptop

192.168.1.10

↓

Phone

192.168.1.20

↓

TV

192.168.1.30
```

Everything inside:

```text
192.168.1.0/24
```

---

# Visual 13: Linux Same Subnet Decision

Linux asks:

```text
Destination?

↓

192.168.1.20

↓

Same Network?

↓

YES

↓

Direct Communication
```

Visualization:

```text
Laptop

↓

Switch

↓

Printer
```

No router.

---

# Visual 14: Different Subnet Decision

Linux asks:

```text
Destination?

↓

8.8.8.8

↓

Same Network?

↓

NO

↓

Gateway
```

Visualization:

```text
Laptop

↓

Router

↓

Internet

↓

8.8.8.8
```

---

# Visual 15: Linux Internal Decision Tree ⭐⭐⭐⭐⭐

```text
Application

↓

TCP

↓

IP

↓

Routing Table

↓

Subnet Check

↓

Same Network?

↓

YES

↓

ARP

↓

MAC

↓

Destination


NO

↓

Gateway

↓

Router

↓

Internet
```

---

# Visual 16: AWS VPC

```text
10.0.0.0/16

↓

Public

10.0.1.0/24

↓

Application

10.0.2.0/24

↓

Database

10.0.3.0/24
```

---

# Visual 17: Kubernetes

```text
Cluster

10.244.0.0/16

↓

Node 1

↓

Pods

↓

Node 2

↓

Pods

↓

Node 3

↓

Pods
```

---

# Visual 18: Docker

```text
Host

↓

docker0

172.17.0.1

↓

Container A

172.17.0.2

↓

Container B

172.17.0.3
```

---

# Visual 19: Security Segmentation

Bad:

```text
Internet

↓

Database
```

---

Good:

```text
Internet

↓

Load Balancer

↓

Application

↓

Database
```

Each layer:

```text
Different Subnet
```

---

# Visual 20: Network Growth Planning ⭐⭐⭐⭐⭐

Bad:

```text
50 Users

↓

/26

62 usable

↓

No growth room
```

---

Good:

```text
50 Users

↓

/25

126 usable

↓

Future Growth
```

---

# Visual 21: Subnet Design Workflow

Always think like this.

```text
How many devices?

↓

Future growth?

↓

Security needs?

↓

Broadcast limits?

↓

Choose subnet
```

---

# Visual 22: Prefix Memory Pyramid ⭐⭐⭐⭐⭐

```text
/8

16 Million


/16

65 Thousand


/24

256


/25

128


/26

64


/27

32


/28

16


/29

8


/30

4
```

---

# Visual 23: Subnetting Lifecycle ⭐⭐⭐⭐⭐

```text
Large Network

↓

Analyze

↓

Split

↓

Organize

↓

Secure

↓

Scale

↓

Troubleshoot

↓

Grow
```

---

# Visual 24: Engineer Mindset

Never think:

```text
Math
```

Think:

```text
Network Design
```

---

# Visual 25: Ultimate Subnetting Mental Model ⭐⭐⭐⭐⭐

Remember this forever.

```text
Large Network

↓

Smaller Networks

↓

Less Broadcast

↓

Better Security

↓

Better Performance

↓

Better Troubleshooting

↓

Scalable Infrastructure
```

---

# Fast Memory Rules

Rule 1

```text
Bigger Prefix

↓

Smaller Network
```

---

Rule 2

```text
More Network Bits

↓

Less Host Bits
```

---

Rule 3

```text
Same Subnet

↓

No Router
```

---

Rule 4

```text
Different Subnet

↓

Use Gateway
```

---

Rule 5

```text
Subnetting

↓

Organization
```

---

# Recommended Reading Order

```text
subnetting.md

↓

subnetting-visuals.md ⭐⭐⭐⭐⭐

↓

subnetting-examples.md ⭐⭐⭐⭐⭐

↓

subnetting-cheatsheet.md ⭐⭐⭐⭐⭐

↓

cidr.md
```
