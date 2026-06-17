# Linux VLAN

# Building Multiple Networks On One Physical Infrastructure

---

# Why This File Exists

Imagine a company.

```text
Engineering Team

HR Team

Finance Team

Security Team
```

Question:

> Should every department have its own physical switch and cables?

That would be expensive.

Instead, we create multiple logical networks.

That's exactly what VLAN does.

---

# Learning Goals

After this file, you should understand:

* Why VLAN exists
* How VLAN works
* VLAN tagging
* VLAN IDs
* Access ports
* Trunk ports
* Native VLAN
* Inter-VLAN routing
* Linux VLAN interfaces
* Docker/Kubernetes usage
* Production architecture

---

# Big Picture

VLAN = Virtual LAN

> One physical network → Multiple isolated logical networks

---

# Mental Model

Think of a building.

Without VLAN:

```text
Everyone shares one giant room.
```

With VLAN:

```text
The same building

↓

Separate departments

↓

Separate access
```

---

# Problem Without VLAN

```mermaid
graph LR

A[Engineering]

B[HR]

C[Finance]

D[Security]

A --- SW[Switch]

B --- SW

C --- SW

D --- SW

SW --- R[Router]
```

Everyone belongs to one broadcast domain.

Problems:

* Too much broadcast traffic
* Poor security
* Difficult management

---

# Solution: VLAN

```mermaid
graph TD

SW[Physical Switch]

SW --> V10[VLAN 10<br/>Engineering]

SW --> V20[VLAN 20<br/>HR]

SW --> V30[VLAN 30<br/>Finance]

SW --> V40[VLAN 40<br/>Security]
```

One switch.

Four independent networks.

---

# VLAN Architecture

```mermaid
graph TD

ENG[Engineering PC]

HR[HR PC]

FIN[Finance PC]

SEC[Security PC]

ENG --> V10[VLAN 10]

HR --> V20[VLAN 20]

FIN --> V30[VLAN 30]

SEC --> V40[VLAN 40]

V10 --> SW[Physical Switch]

V20 --> SW

V30 --> SW

V40 --> SW

SW --> RTR[Router]
```

---

# Broadcast Domains

Without VLAN

```mermaid
graph TD

A[Device A]

B[Device B]

C[Device C]

D[Device D]

A --> SW[Switch]

B --> SW

C --> SW

D --> SW

SW --> ALL[One Broadcast Domain]
```

Everyone hears broadcasts.

---

# With VLAN

```mermaid
graph TD

V10[VLAN 10]

V20[VLAN 20]

V30[VLAN 30]

A[PC A]

B[PC B]

C[PC C]

A --> V10

B --> V20

C --> V30
```

Broadcasts stay inside VLAN.

---

# What Is A VLAN Tag?

Linux and switches insert metadata.

Standard:

```text
IEEE 802.1Q
```

---

# Ethernet Frame

Normal:

```text
Destination MAC

Source MAC

Payload
```

VLAN:

```text
Destination MAC

Source MAC

VLAN Tag

Payload
```

---

# VLAN Tag Structure

```mermaid
flowchart LR

DMAC[Destination MAC]

SMAC[Source MAC]

TAG[802.1Q Tag]

PAYLOAD[Payload]

DMAC --> SMAC

SMAC --> TAG

TAG --> PAYLOAD
```

---

# 802.1Q Internals

```mermaid
graph LR

TAG[802.1Q Tag]

TAG --> PCP[Priority 3 bits]

TAG --> DEI[Drop Eligibility]

TAG --> VID[VLAN ID 12 bits]
```

---

# VLAN ID Range

```text
0 - Reserved

1 - Default VLAN

2 - 4094 Available

4095 Reserved
```

---

# Access Port

Access ports belong to one VLAN.

Example:

```mermaid
graph TD

PC[Engineering PC]

SW[Switch]

V10[VLAN 10]

PC --> SW

SW --> V10
```

---

# Trunk Port

Trunk ports carry multiple VLANs.

```mermaid
graph TD

SW1[Switch 1]

SW2[Switch 2]

V10[VLAN10]

V20[VLAN20]

V30[VLAN30]

SW1 --- SW2

V10 --> SW1

V20 --> SW1

V30 --> SW1
```

---

# Visualizing Trunking

```mermaid
flowchart LR

V10[VLAN10]

V20[VLAN20]

V30[VLAN30]

V10 --> T[Trunk]

V20 --> T

V30 --> T

T --> SW2[Switch 2]
```

One cable.

Multiple networks.

---

# Native VLAN

Special VLAN.

Untagged traffic enters here.

Usually:

```text
VLAN 1
```

Production advice:

```text
Never use VLAN 1 for production workloads.
```

---

# Inter-VLAN Communication Problem

Suppose:

```text
VLAN10 = 10.0.10.0/24

VLAN20 = 10.0.20.0/24
```

Question:

Can they communicate?

No.

Need a router.

---

# Inter-VLAN Routing

```mermaid
graph TD

V10[VLAN 10]

V20[VLAN 20]

RTR[Router]

V10 --> RTR

V20 --> RTR
```

---

# Packet Journey

Engineering → Finance

```mermaid
sequenceDiagram

participant E as Engineering

participant SW as Switch

participant RTR as Router

participant F as Finance

E->>SW: Packet VLAN10

SW->>RTR: Send

RTR->>SW: Route to VLAN30

SW->>F: Deliver
```

---

# Linux VLAN Internals

Linux creates virtual interfaces.

Example:

```bash
sudo ip link add link eth0 name eth0.10 type vlan id 10
```

This means:

```text
Physical NIC = eth0

Virtual VLAN Interface = eth0.10
```

---

# Linux Architecture

```mermaid
graph TD

ETH[eth0]

ETH --> V10[eth0.10]

ETH --> V20[eth0.20]

ETH --> V30[eth0.30]
```

One NIC.

Multiple virtual networks.

---

# Configure VLAN

Create:

```bash
sudo ip link add link eth0 name eth0.10 type vlan id 10
```

Assign IP:

```bash
sudo ip addr add 10.0.10.1/24 dev eth0.10
```

Bring up:

```bash
sudo ip link set eth0.10 up
```

Verify:

```bash
ip -d link
```

---

# Production Data Center

```mermaid
graph TD

SRV1[Web Servers]

SRV2[Database Servers]

SRV3[Monitoring]

SW[Switch]

RTR[Router]

SRV1 --> V10[VLAN10]

SRV2 --> V20[VLAN20]

SRV3 --> V30[VLAN30]

V10 --> SW

V20 --> SW

V30 --> SW

SW --> RTR
```

---

# Kubernetes Relationship

Important:

Kubernetes does **not** create VLANs by default.

But many infrastructures use them.

Example:

```text
Node Management VLAN

Storage VLAN

Pod VLAN

Monitoring VLAN
```

---

# Cloud Relationship

Cloud providers abstract this.

Underneath:

```text
AWS

Azure

GCP
```

still use segmentation concepts.

---

# Security Benefits

```mermaid
graph TD

USER[Compromised Machine]

USER --> V10[VLAN10]

V20[VLAN20]

V30[VLAN30]

USER -. blocked .-> V20

USER -. blocked .-> V30
```

Segmentation limits blast radius.

---

# Production Troubleshooting Flow

```mermaid
flowchart TD

START[Communication Failed]

START --> TAG[VLAN Tag Correct?]

TAG -->|No| FIX1[Fix Switch Config]

TAG -->|Yes| ROUTE[Inter-VLAN Route Exists?]

ROUTE -->|No| FIX2[Configure Router]

ROUTE -->|Yes| FIREWALL[Firewall Blocking?]

FIREWALL -->|Yes| FIX3[Fix Rules]

FIREWALL -->|No| MTU[Check MTU]

MTU --> SUCCESS[Working]
```

---

# Essential Commands

Create VLAN

```bash
ip link add link eth0 name eth0.10 type vlan id 10
```

Show VLANs

```bash
ip -d link
```

Delete VLAN

```bash
ip link delete eth0.10
```

Bring interface up

```bash
ip link set eth0.10 up
```

Show addresses

```bash
ip addr
```

---

# Common Misconceptions

### Misconception 1

> VLAN is a Linux feature

Wrong.

VLAN is an Ethernet standard.

---

### Misconception 2

> VLAN provides security

Partially true.

It's segmentation.

Not complete security.

---

### Misconception 3

> VLANs are obsolete because cloud exists

Wrong.

Cloud infrastructure heavily relies on segmentation concepts.

---

# Engineer Mental Model

Never think:

```text
One cable = One network
```

Think:

```mermaid
graph TD

ONE[One Physical Network]

ONE --> MANY[Many Logical Networks]

MANY --> ISOLATION[Isolation]

ISOLATION --> SECURITY[Security]

SECURITY --> SCALABILITY[Scalability]
```

---

# Capability Checklist

After this file you should understand:

✅ VLAN

✅ 802.1Q

✅ VLAN IDs

✅ Access ports

✅ Trunk ports

✅ Native VLAN

✅ Inter-VLAN routing

✅ Linux VLAN interfaces

✅ Production segmentation

be one of the most important files of the entire repository because VXLAN is the foundation of modern Kubernetes, cloud networking, and multi-host container networking.**
