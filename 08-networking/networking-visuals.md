# Networking Visuals

> This file contains visual explanations of important networking concepts using Mermaid diagrams. The goal is to build intuition rather than memorize definitions.

---

# Table of Contents

1. Network Communication Overview
2. Client-Server Architecture
3. OSI Model
4. TCP/IP Model
5. Packet Journey
6. DNS Resolution
7. HTTP Request Flow
8. TCP Three-Way Handshake
9. TCP Four-Way Termination
10. SSH Connection Flow
11. Routing
12. NAT
13. Home Network Architecture
14. Cloud Architecture
15. Reverse Proxy
16. Load Balancer
17. Firewall
18. Docker Networking
19. Kubernetes Networking

---

# 1. Network Communication Overview

Everything starts here.

```mermaid
graph LR

A[Client]

B[DNS]

C[Internet]

D[Server]

A --> B

B --> C

C --> D
```

Flow:

```text
Client
 ↓
DNS
 ↓
Internet
 ↓
Server
```

---

# 2. Client Server Architecture

```mermaid
graph TD

Client1

Client2

Client3

Server

Client1 --> Server

Client2 --> Server

Client3 --> Server
```

Examples:

```text
Browser → Website

Mobile App → Backend API

Laptop → Database Server
```

---

# 3. OSI Model

```mermaid
graph TD

A[Application Layer]

B[Presentation Layer]

C[Session Layer]

D[Transport Layer]

E[Network Layer]

F[Data Link Layer]

G[Physical Layer]

A --> B

B --> C

C --> D

D --> E

E --> F

F --> G
```

Memory Trick:

```text
Application
Presentation
Session
Transport
Network
Data Link
Physical
```

---

# 4. TCP/IP Model

```mermaid
graph TD

A[Application]

B[Transport]

C[Internet]

D[Network Access]

A --> B

B --> C

C --> D
```

---

# 5. OSI vs TCP/IP

```mermaid
graph LR

subgraph OSI

A1[Application]

A2[Presentation]

A3[Session]

A4[Transport]

A5[Network]

A6[Data Link]

A7[Physical]

end

subgraph TCPIP

B1[Application]

B2[Transport]

B3[Internet]

B4[Network Access]

end

A1 --> B1

A2 --> B1

A3 --> B1

A4 --> B2

A5 --> B3

A6 --> B4

A7 --> B4
```

---

# 6. Packet Journey

```mermaid
graph LR

Laptop

Router

ISP

Internet

Datacenter

Server

Laptop --> Router

Router --> ISP

ISP --> Internet

Internet --> Datacenter

Datacenter --> Server
```

---

# 7. DNS Resolution

```mermaid
sequenceDiagram

participant User

participant Browser

participant DNS

participant Server

User->>Browser: google.com

Browser->>DNS: Resolve domain

DNS-->>Browser: IP Address

Browser->>Server: Connect

Server-->>Browser: Response
```

---

# 8. HTTP Request Flow

```mermaid
sequenceDiagram

participant Browser

participant Server

Browser->>Server: HTTP Request

Server-->>Browser: HTML

Browser->>Server: CSS

Server-->>Browser: CSS File

Browser->>Server: JS

Server-->>Browser: JS File
```

---

# 9. TCP Three-Way Handshake

```mermaid
sequenceDiagram

participant Client

participant Server

Client->>Server: SYN

Server->>Client: SYN ACK

Client->>Server: ACK
```

Visual:

```text
Client            Server

SYN ------------>

<------ SYN ACK

ACK ------------>
```

Connection Established.

---

# 10. TCP Four-Way Termination

```mermaid
sequenceDiagram

participant Client

participant Server

Client->>Server: FIN

Server->>Client: ACK

Server->>Client: FIN

Client->>Server: ACK
```

---

# 11. SSH Connection

```mermaid
sequenceDiagram

participant User

participant SSHClient

participant SSHServer

User->>SSHClient: ssh user@server

SSHClient->>SSHServer: Authentication

SSHServer-->>SSHClient: Success

SSHClient-->>User: Terminal Access
```

---

# 12. Routing

```mermaid
graph LR

A[PC]

B[Router1]

C[Router2]

D[Router3]

E[Server]

A --> B

B --> C

C --> D

D --> E
```

Routers decide where packets should go.

---

# 13. NAT

```mermaid
graph LR

Laptop

Phone

Router

Internet

Laptop --> Router

Phone --> Router

Router --> Internet
```

Visual:

```text
192.168.1.10

192.168.1.20

↓

Public IP

↓

Internet
```

---

# 14. Home Network Architecture

```mermaid
graph TD

Internet

Router

Laptop

Phone

TV

Tablet

Internet --> Router

Router --> Laptop

Router --> Phone

Router --> TV

Router --> Tablet
```

---

# 15. Reverse Proxy

```mermaid
graph LR

User

Nginx

App1

App2

App3

User --> Nginx

Nginx --> App1

Nginx --> App2

Nginx --> App3
```

Benefits:

```text
Security

Load Distribution

SSL Termination
```

---

# 16. Load Balancer

```mermaid
graph TD

Users

LB

Server1

Server2

Server3

Users --> LB

LB --> Server1

LB --> Server2

LB --> Server3
```

Purpose:

```text
Distribute Traffic
```

---

# 17. Firewall

```mermaid
graph LR

Internet

Firewall

Server

Internet --> Firewall

Firewall --> Server
```

Firewall filters traffic.

---

# 18. Docker Networking

```mermaid
graph TD

Container1

Container2

DockerBridge

Internet

Container1 --> DockerBridge

Container2 --> DockerBridge

DockerBridge --> Internet
```

---

# 19. Kubernetes Networking

```mermaid
graph TD

User

Service

Pod1

Pod2

Pod3

User --> Service

Service --> Pod1

Service --> Pod2

Service --> Pod3
```

---

# 20. Full Modern Web Architecture

```mermaid
graph TD

User

DNS

CDN

LoadBalancer

ReverseProxy

API

Database

Cache

User --> DNS

DNS --> CDN

CDN --> LoadBalancer

LoadBalancer --> ReverseProxy

ReverseProxy --> API

API --> Database

API --> Cache
```

---

# 21. Networking Mental Model

```text
Application

↓

Protocol

↓

Packet

↓

IP Address

↓

Router

↓

Internet

↓

Router

↓

Destination

↓

Application
```

---

# Recommended Study Order

```text
Client Server

↓

OSI

↓

TCP/IP

↓

DNS

↓

TCP

↓

HTTP

↓

Routing

↓

NAT

↓

SSH

↓

Reverse Proxy

↓

Load Balancer

↓

Docker Networking

↓

Kubernetes Networking
```
