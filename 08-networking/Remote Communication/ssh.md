# SSH (Secure Shell)

# 1. What is SSH?

SSH (Secure Shell) is a **secure encrypted communication protocol** used to remotely access and manage another computer over an untrusted network.

Think of SSH as:

> **A secure tunnel that allows you to control another computer from anywhere.**

Without SSH:

```text
Your commands
     ↓
Internet (Visible to everyone)
     ↓
Remote Server
```

With SSH:

```text
Your commands
     ↓
Encrypted Tunnel
     ↓
Remote Server
```

Nobody can easily read, modify, or impersonate the communication.

---

# 2. Why SSH Exists

Early systems used:

```text
telnet
rlogin
rsh
```

Problem:

```text
Username: admin
Password: mypassword123
```

Everything was sent as plain text.

Attackers could intercept traffic.

SSH solved this by introducing:

* Encryption
* Authentication
* Integrity verification
* Secure tunneling

---

# 3. Where SSH is Used

SSH is everywhere.

### Developers

```text
Laptop → Cloud Server
```

Example:

```bash
ssh ubuntu@server-ip
```

---

### DevOps

```text
CI/CD Server → Production Servers
```

---

### SRE

```text
Incident → Login → Investigate
```

---

### Kubernetes Engineers

```text
Laptop
   ↓
Bastion Host
   ↓
Cluster Nodes
```

---

### Security Engineers

```text
Auditing
Penetration Testing
Hardening
Monitoring
```

---

### Founders

Managing servers without physical access.

---

# 4. Mental Model

Imagine SSH as 4 layers.

```text
┌───────────────────────┐
│ User Authentication   │
├───────────────────────┤
│ Encryption            │
├───────────────────────┤
│ Secure Channel        │
├───────────────────────┤
│ TCP Transport         │
└───────────────────────┘
```

---

# 5. Default Port

SSH uses:

```text
TCP 22
```

Check:

```bash
ss -tulnp | grep 22
```

Example:

```text
tcp LISTEN 0 128 *:22 *:*
```

---

# 6. Basic SSH Architecture

```mermaid
flowchart LR

A[Laptop]
B[Internet]
C[SSH Server]

A -->|Encrypted Communication| B
B --> C
```

---

# 7. Client-Server Model

SSH follows a client-server architecture.

## SSH Client

Runs on your machine.

Examples:

```bash
ssh
scp
sftp
rsync
```

---

## SSH Server

Daemon running on remote machine.

```bash
sshd
```

Check:

```bash
systemctl status ssh

# Ubuntu
systemctl status ssh

# RHEL
systemctl status sshd
```

---

# 8. Components of SSH

```text
SSH

├── Transport Layer
├── Authentication Layer
├── Connection Layer
└── Channel Multiplexing
```

---

# 9. What Happens During SSH Connection?

Suppose:

```bash
ssh ubuntu@10.0.0.15
```

Sequence:

```mermaid
sequenceDiagram

participant Client
participant Server

Client->>Server: TCP Connection (22)

Server->>Client: Server Public Key

Client->>Server: Key Exchange

Client->>Server: Authentication

Server->>Client: Session Established
```

---

# 10. SSH Handshake Deep Dive

There are multiple phases.

## Phase 1

TCP connection

```text
Client
  ↓
Server:22
```

---

## Phase 2

Exchange algorithms.

Example:

```text
Encryption Algorithm

AES256

Key Exchange

curve25519

MAC

HMAC-SHA256
```

---

## Phase 3

Generate session keys.

---

## Phase 4

Authenticate user.

---

## Phase 5

Create secure shell.

---

# 11. SSH Authentication Methods

Several methods exist.

## Password Authentication

```bash
ssh user@server
```

Prompts:

```text
Password:
```

Simple but weaker.

---

## Public Key Authentication

Most common.

```text
Private Key
     ↓
Public Key
```

Public key lives on server.

Private key stays with user.

---

## Certificate Authentication

Used by large organizations.

```text
CA
 ↓
Signs User Certificates
```

Examples:

```text
Google
Netflix
Uber
Cloudflare
```

---

## Hardware Authentication

Examples:

```text
YubiKey
FIDO2
Security Tokens
```

---

# 12. Public Key Authentication Visual

```mermaid
flowchart LR

A[Private Key<br>Local Laptop]

B[Public Key<br>Server]

A -->|Mathematical Relationship| B
```

Important:

```text
Public key can be shared.

Private key must NEVER be shared.
```

---

# 13. Generate SSH Keys

Modern algorithm:

```bash
ssh-keygen -t ed25519
```

Output:

```text
~/.ssh/id_ed25519

~/.ssh/id_ed25519.pub
```

---

# 14. RSA vs ED25519

| Feature  | RSA    | ED25519   |
| -------- | ------ | --------- |
| Speed    | Medium | Fast      |
| Key Size | Large  | Small     |
| Security | Good   | Excellent |
| Modern   | Older  | Preferred |

Recommendation:

```text
Use ED25519
```

---

# 15. Copy Public Key to Server

Method 1:

```bash
ssh-copy-id ubuntu@server-ip
```

Method 2:

Append manually.

```bash
~/.ssh/authorized_keys
```

---

# 16. Important SSH Files

## Client Side

```text
~/.ssh/

├── config
├── known_hosts
├── id_ed25519
└── id_ed25519.pub
```

---

## Server Side

```text
/etc/ssh/

├── sshd_config
├── ssh_host_ed25519_key
└── ssh_host_ed25519_key.pub
```

---

# 17. `known_hosts`

Stores trusted servers.

Example:

```text
github.com ssh-ed25519 AAAAC3...
```

Purpose:

Prevent MITM attacks.

---

# 18. `authorized_keys`

Stores allowed users.

Example:

```text
ssh-ed25519 AAAAC3N...
```

Only matching private keys can login.

---

# 19. SSH Config File

Location:

```bash
~/.ssh/config
```

Example:

```text
Host production

HostName 54.10.20.30

User ubuntu

IdentityFile ~/.ssh/prod.pem

Port 22
```

Now:

```bash
ssh production
```

instead of:

```bash
ssh -i ~/.ssh/prod.pem ubuntu@54.10.20.30
```

---

# 20. SSH Connection Flow

```mermaid
flowchart TD

A[User Types SSH]

B[DNS Lookup]

C[TCP Connection]

D[Key Exchange]

E[Verify Server]

F[Authenticate User]

G[Create Session]

H[Interactive Shell]

A --> B
B --> C
C --> D
D --> E
E --> F
F --> G
G --> H
```

---

# 21. Bastion Host Architecture

Production systems rarely expose all servers.

```mermaid
flowchart LR

A[Laptop]

B[Bastion Host]

C[Private Server 1]

D[Private Server 2]

E[Private Server 3]

A --> B

B --> C

B --> D

B --> E
```

---

# 22. SSH Agent

Problem:

```text
Multiple servers
Multiple private keys
```

Solution:

```text
ssh-agent
```

Start:

```bash
eval "$(ssh-agent -s)"
```

Add key:

```bash
ssh-add ~/.ssh/id_ed25519
```

---

# 23. SSH Port Forwarding

SSH can create tunnels.

Types:

```text
Local Forwarding

Remote Forwarding

Dynamic Forwarding
```

---

# 24. Local Port Forwarding

```mermaid
flowchart LR

A[Browser]

B[Localhost:8080]

C[SSH Tunnel]

D[Remote DB:5432]

A --> B

B --> C

C --> D
```

Command:

```bash
ssh -L 8080:localhost:5432 user@server
```

---

# 25. Remote Port Forwarding

Expose local application remotely.

```bash
ssh -R 9000:localhost:3000 user@server
```

---

# 26. Dynamic Forwarding

Acts like SOCKS proxy.

```bash
ssh -D 1080 user@server
```

Applications:

```text
Browser Proxy
Secure Browsing
Internal Network Access
```

---

# 27. SSH Multiplexing

Problem:

Every SSH connection repeats handshake.

Solution:

Reuse connections.

```text
Laptop

   ↓

Master Connection

 ↓     ↓      ↓

SSH1 SSH2 SSH3
```

Config:

```text
ControlMaster auto

ControlPath ~/.ssh/socket-%r@%h:%p

ControlPersist 10m
```

---

# 28. Production Security Hardening

Disable root login.

```text
PermitRootLogin no
```

Disable passwords.

```text
PasswordAuthentication no
```

Disable empty passwords.

```text
PermitEmptyPasswords no
```

Use modern algorithms.

Enable firewall.

Restrict users.

```text
AllowUsers devops sre admin
```

---

# 29. Modern SSH Architecture

```mermaid
flowchart TD

A[Engineer Laptop]

B[VPN]

C[Bastion]

D[Production VPC]

E[Application Servers]

F[Database Servers]

G[Kubernetes Nodes]

A --> B

B --> C

C --> D

D --> E

D --> F

D --> G
```

---

# 30. Common SSH Commands

### Connect

```bash
ssh user@host
```

### Specific Port

```bash
ssh -p 2222 user@host
```

### Use Key

```bash
ssh -i key.pem user@host
```

### Verbose Mode

```bash
ssh -v user@host
```

Very verbose:

```bash
ssh -vvv user@host
```

---

# 31. SSH Troubleshooting Flow

```mermaid
flowchart TD

A[SSH Fails]

B[Network Reachable?]

C[Port 22 Open?]

D[SSH Service Running?]

E[Firewall Blocking?]

F[Authentication Problem?]

G[Known Hosts Problem?]

H[Success]

A --> B

B --> C

C --> D

D --> E

E --> F

F --> G

G --> H
```

---

# 32. Troubleshooting Commands

Check connectivity:

```bash
ping server-ip
```

Check port:

```bash
nc -zv server-ip 22
```

Check daemon:

```bash
systemctl status sshd
```

Check logs:

Ubuntu:

```bash
sudo journalctl -u ssh
```

RHEL:

```bash
sudo journalctl -u sshd
```

Debug:

```bash
ssh -vvv user@host
```

---

# 33. Real World Examples

### GitHub

```bash
git clone git@github.com:user/repo.git
```

Uses SSH.

---

### AWS EC2

```bash
ssh -i key.pem ubuntu@ec2-ip
```

---

### Kubernetes Nodes

```bash
ssh worker-node
```

---

### CI/CD

```text
GitHub Actions

↓

SSH

↓

Production Deployment
```

---

# 34. Interview Questions

### Beginner

* What is SSH?
* Why is SSH more secure than Telnet?
* What is port 22?

### Intermediate

* Difference between `known_hosts` and `authorized_keys`?
* Explain SSH handshake.
* Explain SSH agent.

### Advanced

* How does bastion architecture work?
* How would you secure SSH in production?
* Explain local vs remote vs dynamic forwarding.
* How would you investigate a compromised SSH server?

---

# 35. Key Takeaways

```text
SSH = Secure Remote Communication

SSH = Encryption + Authentication + Integrity

TCP Port = 22

Preferred Keys = ED25519

Main Daemon = sshd

Key Files:

known_hosts
authorized_keys
config

Advanced Concepts:

Bastion Host
SSH Agent
Port Forwarding
Multiplexing
Production Hardening
```

`scp.md` should build on SSH and explain secure file transfer internals and production usage.
