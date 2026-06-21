# Docker Visual Atlas

> "Docker is easier to understand when you stop reading text and start seeing systems."

---

# 1. Docker Big Picture

```mermaid
flowchart TD

A[Developer]

B[Docker CLI]

C[Docker Engine]

D[containerd]

E[runc]

F[Linux Kernel]

G[Container]

A --> B

B --> C

C --> D

D --> E

E --> F

F --> G
```

---

# 2. Docker Mental Model

```mermaid
flowchart LR

A[Code]

B[Dockerfile]

C[Image]

D[Container]

A --> B

B --> C

C --> D
```

---

# 3. Docker Architecture

```mermaid
flowchart TD

subgraph User Space

A[Docker CLI]

B[Docker API]

end

subgraph Docker Host

C[Docker Daemon]

D[Images]

E[Networks]

F[Volumes]

G[Containers]

end

subgraph Runtime Layer

H[containerd]

I[runc]

end

subgraph Linux

J[Namespaces]

K[Cgroups]

L[OverlayFS]

end

A --> C

B --> C

C --> D

C --> E

C --> F

C --> G

C --> H

H --> I

I --> J

I --> K

I --> L
```

---

# 4. Complete `docker run` Flow

```mermaid
sequenceDiagram

participant User

participant CLI

participant Docker

participant containerd

participant runc

participant Linux

User->>CLI: docker run nginx

CLI->>Docker: Request

Docker->>containerd: Create Container

containerd->>runc: Execute

runc->>Linux: Create Namespaces

runc->>Linux: Create Cgroups

runc->>Linux: Mount OverlayFS

runc->>Linux: Start nginx

Linux->>User: Running Container
```

---

# 5. Docker Ecosystem

```mermaid
mindmap

root((Docker))

    Docker Engine

    Docker Images

    Docker Layers

    Dockerfiles

    Docker Networking

    Docker Volumes

    Docker Compose

    Registries

    Security
```

---

# 6. Image To Container Flow

```mermaid
flowchart LR

A[Dockerfile]

B[Docker Build]

C[Docker Image]

D[Docker Run]

E[Container]

A --> B

B --> C

C --> D

D --> E
```

---

# 7. Docker Image Internals

```mermaid
flowchart TD

A[Ubuntu]

B[Node]

C[NPM Dependencies]

D[Application]

E[Final Image]

A --> E

B --> E

C --> E

D --> E
```

---

# 8. Docker Layers

```mermaid
flowchart TD

A[Base OS]

B[Runtime]

C[Libraries]

D[Application]

E[Writable Layer]

F[Running Container]

A --> F

B --> F

C --> F

D --> F

E --> F
```

---

# 9. Copy-On-Write

```mermaid
flowchart TD

A[Read Only Layers]

B[Writable Layer]

C[Merged Filesystem]

A --> C

B --> C
```

---

# 10. Docker Build Cache

```mermaid
flowchart TD

A[FROM]

B[RUN apt install]

C[COPY package.json]

D[RUN npm install]

E[COPY source]

A --> B

B --> C

C --> D

D --> E
```

---

# 11. Good Dockerfile Flow

```mermaid
flowchart TD

A[Base Image]

B[Copy package.json]

C[Install Dependencies]

D[Copy Source]

E[Build]

A --> B

B --> C

C --> D

D --> E
```

---

# 12. Multi Stage Build

```mermaid
flowchart LR

subgraph Builder

A[Node]

B[npm install]

C[npm run build]

end

subgraph Runtime

D[Nginx]

E[Copy Build Files]

end

A --> B

B --> C

C --> E

D --> E
```

---

# 13. Image Registry Flow

```mermaid
flowchart LR

A[Developer]

B[Build]

C[Docker Hub]

D[Production]

A --> B

B --> C

C --> D
```

---

# 14. Volume Architecture

```mermaid
flowchart TD

A[Container]

B[Docker Volume]

C[Host Disk]

A --> B

B --> C
```

---

# 15. Bind Mount Architecture

```mermaid
flowchart LR

A[Local Folder]

B[Container]

A --> B
```

---

# 16. Volume Use Cases

```mermaid
mindmap

root((Volumes))

    Databases

    Uploads

    Logs

    Cache

    Shared Data
```

---

# 17. Docker Networking Big Picture

```mermaid
flowchart TD

A[Container A]

B[Container B]

C[docker0]

D[Host]

E[Internet]

A --> C

B --> C

C --> D

D --> E
```

---

# 18. veth Pair Architecture

```mermaid
flowchart LR

A[Container]

B[veth]

C[docker0]

D[Host]

A --> B

B --> C

C --> D
```

---

# 19. Port Mapping

```mermaid
flowchart LR

A[Browser]

B[Host Port 8080]

C[Container Port 80]

A --> B

B --> C
```

---

# 20. Docker DNS

```mermaid
flowchart TD

A[Frontend]

B[Backend]

C[Database]

A --> B

B --> C
```

Instead of:

```text
172.18.0.5
```

Docker uses:

```text
backend

database
```

---

# 21. Docker Compose Architecture

```mermaid
flowchart TD

A[docker-compose.yml]

B[Frontend]

C[Backend]

D[Redis]

E[PostgreSQL]

F[Internal Network]

A --> B

A --> C

A --> D

A --> E

B --> F

C --> F

D --> F

E --> F
```

---

# 22. Full Stack Application

```mermaid
flowchart TD

A[User]

B[Nginx]

C[Frontend]

D[Backend]

E[Redis]

F[PostgreSQL]

A --> B

B --> C

C --> D

D --> E

D --> F
```

---

# 23. Production Docker Stack

```mermaid
flowchart TD

A[Users]

B[Load Balancer]

C[Nginx]

D[Containers]

E[Redis]

F[PostgreSQL]

G[Prometheus]

H[Grafana]

I[ELK]

A --> B

B --> C

C --> D

D --> E

D --> F

D --> G

G --> H

D --> I
```

---

# 24. Docker Security Layers

```mermaid
flowchart TD

A[Source Code]

B[Dockerfile]

C[Image]

D[Registry]

E[Runtime]

F[Linux]

A --> B

B --> C

C --> D

D --> E

E --> F
```

---

# 25. Image Security Pipeline

```mermaid
flowchart TD

A[Code]

B[Build]

C[Scan]

D[SBOM]

E[Sign]

F[Registry]

A --> B

B --> C

C --> D

D --> E

E --> F
```

---

# 26. Runtime Security

```mermaid
flowchart TD

A[Container]

B[eBPF]

C[Detection]

D[Alerts]

E[Response]

A --> B

B --> C

C --> D

D --> E
```

---

# 27. Deployment Pipeline

```mermaid
flowchart TD

A[Git Push]

B[CI]

C[Build Image]

D[Registry]

E[Deploy]

F[Observe]

A --> B

B --> C

C --> D

D --> E

E --> F
```

---

# 28. Blue Green Deployment

```mermaid
flowchart TD

A[Users]

B[Load Balancer]

C[Blue]

D[Green]

A --> B

B --> C

B --> D
```

---

# 29. Canary Deployment

```mermaid
flowchart LR

A[5%]

B[20%]

C[50%]

D[100%]

A --> B

B --> C

C --> D
```

---

# 30. Docker To Kubernetes Evolution

```mermaid
flowchart TD

A[Docker]

B[Docker Compose]

C[Kubernetes]

D[Cloud Native]

A --> B

B --> C

C --> D
```

---

# 31. The Docker Knowledge Graph

```mermaid
mindmap

root((Docker))

    Images

        Layers

        Cache

        Registries

    Containers

        Namespaces

        Cgroups

        OverlayFS

    Dockerfiles

        Multi Stage

        Optimization

    Networking

        docker0

        NAT

        DNS

        veth

    Storage

        Volumes

        Bind Mounts

    Compose

        Services

        Networks

        Volumes

    Security

        Images

        Runtime

        Supply Chain

    Production

        Observability

        Deployments

        Scaling
```

---

# 32. The Entire Docker Stack

```mermaid
flowchart TD

A[Developer]

B[Docker CLI]

C[Docker Engine]

D[containerd]

E[runc]

F[Linux Kernel]

G[Namespaces]

H[Cgroups]

I[OverlayFS]

J[Container]

A --> B

B --> C

C --> D

D --> E

E --> F

F --> G

F --> H

F --> I

G --> J

H --> J

I --> J
```

---

# Final Visual Mental Model

```text
Code

↓

Dockerfile

↓

Docker Image

↓

Docker Engine

↓

containerd

↓

runc

↓

Linux Kernel

↓

Namespaces + Cgroups + OverlayFS

↓

Container

↓

Production Infrastructure
```

> **Docker is not one tool. Docker is a Linux abstraction platform built on decades of operating system engineering.**
