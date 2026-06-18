# Networking Troubleshooting

> Networking troubleshooting is not about memorizing commands. It is a systematic process of narrowing down where communication is failing.

The most important skill:

> **Never guess. Always eliminate possibilities.**

---

# Table of Contents

1. The Troubleshooting Mindset
2. Universal Troubleshooting Flow
3. The Layer-by-Layer Method
4. Golden Questions
5. Essential Linux Commands
6. DNS Troubleshooting
7. Connectivity Troubleshooting
8. Port Troubleshooting
9. Routing Troubleshooting
10. Firewall Troubleshooting
11. HTTP Troubleshooting
12. SSH Troubleshooting
13. Docker Networking Troubleshooting
14. Kubernetes Networking Troubleshooting
15. Production Scenarios
16. Troubleshooting Playbooks

---

# The Troubleshooting Mindset

Never do this:

```text
Website Down

↓

Restart Server

↓

Hope It Works
```

Do this:

```text
Observe

↓

Gather Evidence

↓

Isolate Problem

↓

Verify Root Cause

↓

Fix

↓

Verify Fix
```

---

# The Universal Troubleshooting Flow

This is your master flowchart.

```text
Application Problem?

↓

DNS Working?

↓

IP Reachable?

↓

Route Correct?

↓

Port Open?

↓

Firewall Blocking?

↓

Service Running?

↓

Server Healthy?
```

Memorize this.

---

# The Layer-by-Layer Method

Always troubleshoot from the bottom up.

```text
Layer 1

Physical

↓

Layer 2

Data Link

↓

Layer 3

Network

↓

Layer 4

Transport

↓

Layer 7

Application
```

Never jump randomly.

---

# Layer 1: Physical

Questions:

```text
Is the cable connected?

Is WiFi enabled?

Is the device powered on?

Is the NIC working?
```

Commands:

```bash
ip link
```

Look for:

```text
UP

DOWN
```

---

# Layer 2: Data Link

Questions:

```text
Can devices communicate locally?

Is the switch working?

Are VLANs configured correctly?
```

Commands:

```bash
ip link

arp -a
```

---

# Layer 3: Network Layer

Questions:

```text
Does the machine have an IP?

Is routing correct?

Can packets leave the machine?
```

Commands:

```bash
ip a

ip route

ping
```

---

# Layer 4: Transport Layer

Questions:

```text
Is the port open?

Is something listening?
```

Commands:

```bash
ss -tulpn

netstat -tulpn
```

---

# Layer 7: Application Layer

Questions:

```text
Is the application healthy?

Is configuration correct?

Are dependencies available?
```

Commands:

```bash
curl

journalctl

systemctl status
```

---

# Golden Troubleshooting Questions

Always ask these.

```text
What broke?

When did it break?

What changed?

Who is affected?

Is it reproducible?

Can I isolate it?
```

---

# The Golden Rule

Never ask:

```text
What is broken?
```

Ask:

```text
Where is communication stopping?
```

---

# Step 1: Verify Interface

Check interfaces.

```bash
ip a
```

or

```bash
ip link
```

Healthy:

```text
eth0

UP
```

Bad:

```text
eth0

DOWN
```

---

# Step 2: Verify IP Address

Command:

```bash
ip a
```

Check:

```text
Correct IP?

Correct Subnet?
```

Example:

```text
192.168.1.100/24
```

---

# Step 3: Verify Connectivity

Ping yourself:

```bash
ping 127.0.0.1
```

Ping gateway:

```bash
ping 192.168.1.1
```

Ping internet:

```bash
ping 8.8.8.8
```

---

# Ping Decision Tree

```text
127.0.0.1 Works?

↓

No

↓

Local TCP/IP Problem

↓

Yes

↓

Gateway Reachable?

↓

No

↓

LAN Problem

↓

Yes

↓

Internet Reachable?

↓

No

↓

Routing Problem
```

---

# DNS Troubleshooting

Symptoms:

```text
Internet Works

Website Doesn't
```

Example:

```bash
ping 8.8.8.8
```

Works.

```bash
ping google.com
```

Fails.

DNS issue.

Commands:

```bash
dig google.com

nslookup google.com

cat /etc/resolv.conf
```

---

# DNS Flow

```text
Application

↓

DNS Resolver

↓

Root DNS

↓

TLD

↓

Authoritative DNS

↓

IP Address
```

---

# Port Troubleshooting

Question:

> Is anything listening?

Commands:

```bash
ss -tulpn
```

Example:

```text
22 ssh

80 nginx

443 nginx
```

---

# Port Conflict Example

Problem:

```text
Nginx Won't Start
```

Check:

```bash
ss -tulpn
```

Output:

```text
0.0.0.0:80
```

Another application owns port 80.

---

# Routing Troubleshooting

Command:

```bash
ip route
```

Healthy:

```text
default via 192.168.1.1
```

Questions:

```text
Do routes exist?

Is gateway correct?
```

---

# Trace Network Path

Command:

```bash
traceroute google.com
```

Shows:

```text
PC

↓

Router

↓

ISP

↓

Internet

↓

Server
```

Failure location becomes visible.

---

# Firewall Troubleshooting

Question:

> Is traffic being blocked?

Commands:

```bash
sudo iptables -L

sudo nft list ruleset
```

Look for:

```text
DROP

REJECT
```

---

# HTTP Troubleshooting

Command:

```bash
curl https://example.com
```

Verbose:

```bash
curl -v https://example.com
```

Check:

```text
DNS

TLS

Headers

Response
```

---

# SSH Troubleshooting

Problem:

```text
Cannot SSH
```

Checklist:

```text
Server Reachable?

↓

Port 22 Open?

↓

SSH Service Running?

↓

Firewall Blocking?

↓

Credentials Correct?
```

Commands:

```bash
ssh -v user@server

ss -tulpn

systemctl status ssh
```

---

# Tcpdump Troubleshooting

Capture traffic.

```bash
sudo tcpdump
```

Specific Interface:

```bash
sudo tcpdump -i eth0
```

Specific Port:

```bash
sudo tcpdump port 80
```

Specific Host:

```bash
sudo tcpdump host 8.8.8.8
```

---

# Docker Networking Troubleshooting

Visual:

```text
Container

↓

Docker Bridge

↓

Host

↓

Internet
```

Commands:

List networks:

```bash
docker network ls
```

Inspect:

```bash
docker network inspect bridge
```

Container IP:

```bash
docker inspect container_name
```

---

# Kubernetes Troubleshooting

Visual:

```text
User

↓

Service

↓

Pod

↓

Container
```

Checklist:

```text
Pod Running?

↓

Service Exists?

↓

DNS Working?

↓

Endpoints Healthy?
```

Commands:

```bash
kubectl get pods

kubectl get svc

kubectl get endpoints

kubectl logs pod_name
```

---

# Production Scenario 1

# Website Down

Flow:

```text
Website Down

↓

DNS?

↓

Ping?

↓

Port?

↓

Firewall?

↓

Application?
```

Commands:

```bash
dig

ping

ss

curl

journalctl
```

---

# Production Scenario 2

# SSH Not Working

Flow:

```text
Server Reachable?

↓

Port Open?

↓

SSH Running?

↓

Firewall?

↓

Credentials?
```

Commands:

```bash
ping

ss

systemctl

ssh -v
```

---

# Production Scenario 3

# API Slow

Check:

```text
CPU

Memory

Network

Database

Cache
```

Commands:

```bash
top

free -h

ss

iostat
```

---

# Production Scenario 4

# DNS Outage

Symptoms:

```text
Internet Works

Domains Fail
```

Commands:

```bash
dig

nslookup

cat /etc/resolv.conf
```

---

# The Five Networking Super Commands

If you only remember five commands:

```bash
ip a

ip route

ss -tulpn

ping

dig
```

You'll solve many problems.

---

# The Networking Investigation Pyramid

Always move upward.

```text
Application

↓

DNS

↓

IP

↓

Routing

↓

Port

↓

Firewall

↓

Network Interface
```

---

# The Networking Engineer Mindset

Don't think:

```text
What command should I run?
```

Think:

```text
What assumption am I verifying?
```

Examples:

```text
Assumption:

DNS works

↓

Verify:

dig
```

```text
Assumption:

Port is open

↓

Verify:

ss
```

```text
Assumption:

Route exists

↓

Verify:

ip route
```

---

# Troubleshooting Checklist

```text
✅ Interface Up

✅ IP Assigned

✅ Route Exists

✅ DNS Works

✅ Port Open

✅ Firewall Allows

✅ Service Running

✅ Application Healthy
```

---

# Golden Rule

> Every networking problem is simply a communication problem. Your job is to find exactly where communication stops.

