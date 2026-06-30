# Database Internals

## From SQL Queries to Storage Engines, B-Trees, WAL, MVCC, Replication, and Distributed Databases

---

# Why This Exists

Most engineers use databases every day.

They write:

```sql
SELECT * FROM users;

INSERT INTO orders VALUES (...);

UPDATE products SET stock = stock - 1;
```

and the database simply returns results.

But what actually happens inside?

When you execute:

```sql
SELECT * FROM users WHERE email='vip@example.com';
```

the database performs an incredible amount of work:

```text
Parser
↓
Optimizer
↓
Execution Planner
↓
Index Lookup
↓
Buffer Cache
↓
Storage Engine
↓
Filesystem
↓
Linux Kernel
↓
Disk
```

Understanding database internals is essential for:

* Backend Engineers
* Database Engineers
* DevOps Engineers
* SREs
* Platform Engineers
* System Architects
* Startup Founders

Because most production bottlenecks eventually become:

```text
Database Bottlenecks
```

---

# The Core Mental Model

Most beginners think:

```text
Application
     ↓
Database
     ↓
Result
```

Reality:

```text
Application
     ↓
SQL Parser
     ↓
Optimizer
     ↓
Execution Plan
     ↓
Indexes
     ↓
Memory Cache
     ↓
Storage Engine
     ↓
Filesystem
     ↓
Disk
```

A database is not a file.

A database is:

```text
A Data Processing Engine
```

---

# The Big Picture

```mermaid
flowchart TD

QUERY["SQL Query"]

QUERY --> PARSER["Parser"]

PARSER --> OPT["Optimizer"]

OPT --> PLAN["Execution Plan"]

PLAN --> CACHE["Buffer Cache"]

CACHE --> INDEX["Indexes"]

INDEX --> STORAGE["Storage Engine"]

STORAGE --> DISK["Disk"]
```

---

# Database Architecture Overview

```mermaid
graph TD

CLIENT["Application"]

CLIENT --> DATABASE["Database"]

DATABASE --> PARSER["Parser"]

DATABASE --> OPT["Optimizer"]

DATABASE --> EXEC["Executor"]

DATABASE --> CACHE["Buffer Pool"]

DATABASE --> STORAGE["Storage Engine"]
```

---

# The Journey of a Query

Example:

```sql
SELECT * FROM users WHERE id = 100;
```

The database does not immediately read a file.

Instead:

```text
Parse
Validate
Optimize
Execute
Return
```

---

# Query Lifecycle

```mermaid
flowchart LR

QUERY["Query"]

QUERY --> PARSE["Parse"]

PARSE --> OPT["Optimize"]

OPT --> EXEC["Execute"]

EXEC --> RESULT["Result"]
```

---

# Query Parser

The parser answers:

```text
Is this valid SQL?
```

---

# Parser Architecture

```mermaid
graph TD

SQL["SQL Query"]

SQL --> TOKEN["Tokens"]

TOKEN --> PARSER["Parser"]

PARSER --> AST["Query Tree"]
```

---

# Example

Input:

```sql
SELECT * FROM users;
```

Parser creates:

```text
Abstract Syntax Tree (AST)
```

---

# Query Optimizer

One of the most important database components.

---

# Optimizer Goal

```text
Fastest Query Plan
```

---

# Optimizer Architecture

```mermaid
graph TD

QUERY["Query"]

QUERY --> OPT["Optimizer"]

OPT --> PLAN1["Plan A"]

OPT --> PLAN2["Plan B"]

OPT --> PLAN3["Plan C"]

PLAN2 --> EXEC["Chosen Plan"]
```

---

# Why Optimizers Exist

Consider:

```sql
SELECT * FROM users WHERE id=100;
```

Options:

```text
Scan Entire Table

Use Index
```

Optimizer chooses:

```text
Use Index
```

---

# Execution Engine

Executes the chosen plan.

---

# Execution Architecture

```mermaid
graph TD

PLAN["Execution Plan"]

PLAN --> EXEC["Executor"]

EXEC --> STORAGE["Storage"]

EXEC --> RESULT["Results"]
```

---

# Storage Engine

The storage engine manages data on disk.

---

# Responsibilities

```text
Reads

Writes

Indexes

Transactions

Recovery
```

---

# Storage Engine Architecture

```mermaid
graph TD

DATABASE["Database"]

DATABASE --> STORAGE["Storage Engine"]

STORAGE --> FILES["Data Files"]

FILES --> DISK["Disk"]
```

---

# Database Storage Layout

```text
Database
     ↓
Tables
     ↓
Pages
     ↓
Blocks
     ↓
Disk
```

---

# Why Databases Use Pages

Disks are slow.

Reading one row at a time is inefficient.

Databases read:

```text
Pages
```

instead.

Typically:

```text
4 KB

8 KB

16 KB
```

---

# Page Architecture

```mermaid
graph TD

TABLE["Table"]

TABLE --> PAGE1["Page"]

TABLE --> PAGE2["Page"]

TABLE --> PAGE3["Page"]

PAGE1 --> ROWS["Rows"]
```

---

# B-Tree Indexes

The most important database structure.

---

# Why Indexes Exist

Without index:

```text
Find User
      ↓
Scan Entire Table
```

With index:

```text
Find User
      ↓
Jump Directly
```

---

# B-Tree Architecture

```mermaid
graph TD

ROOT["Root"]

ROOT --> N1["Node"]

ROOT --> N2["Node"]

N1 --> LEAF1["Leaf"]

N1 --> LEAF2["Leaf"]

N2 --> LEAF3["Leaf"]

N2 --> LEAF4["Leaf"]
```

---

# Search Complexity

Table Scan:

```text
O(n)
```

Index Lookup:

```text
O(log n)
```

Huge difference.

---

# Index Lookup Flow

```mermaid
flowchart TD

QUERY["Find Key"]

QUERY --> ROOT["Root"]

ROOT --> BRANCH["Branch"]

BRANCH --> LEAF["Leaf"]

LEAF --> ROW["Row"]
```

---

# Buffer Cache

Reading disks is expensive.

Databases cache frequently accessed pages.

---

# Cache Architecture

```mermaid
graph TD

QUERY["Query"]

QUERY --> CACHE["Buffer Cache"]

CACHE --> HIT["Cache Hit"]

CACHE --> MISS["Cache Miss"]

MISS --> DISK["Disk"]
```

---

# Why Cache Matters

Disk latency:

```text
Milliseconds
```

Memory latency:

```text
Nanoseconds
```

Difference:

```text
Millions of Times Faster
```

---

# Buffer Pool Architecture

```mermaid
graph TD

MEMORY["Buffer Pool"]

MEMORY --> PAGE1["Page"]

MEMORY --> PAGE2["Page"]

MEMORY --> PAGE3["Page"]
```

---

# Write-Ahead Logging (WAL)

One of the most important database concepts.

---

# Problem

What if power fails during a write?

---

# WAL Solution

Before changing data:

```text
Write Log First
```

---

# WAL Architecture

```mermaid
flowchart TD

TRANSACTION["Transaction"]

TRANSACTION --> WAL["WAL Log"]

WAL --> DISK["Disk"]

DISK --> DATA["Data File"]
```

---

# WAL Benefits

```text
Crash Recovery

Durability

Replication

Consistency
```

---

# Crash Recovery

```mermaid
flowchart TD

CRASH["Crash"]

CRASH --> WAL["Read WAL"]

WAL --> RECOVER["Recover Data"]

RECOVER --> DATABASE["Consistent State"]
```

---

# ACID Transactions

Foundation of relational databases.

---

# ACID Model

```mermaid
graph TD

ACID["ACID"]

ACID --> A["Atomicity"]

ACID --> C["Consistency"]

ACID --> I["Isolation"]

ACID --> D["Durability"]
```

---

# Atomicity

```text
All

or

Nothing
```

---

# Example

Transfer money:

```text
Debit Account A

Credit Account B
```

Both succeed or neither succeeds.

---

# Durability

Once committed:

```text
Data Survives Crash
```

Thanks to WAL.

---

# Concurrency Problem

Multiple users access data simultaneously.

---

# Example

```text
User A Updates Row

User B Reads Row
```

How should database behave?

---

# Locking Architecture

```mermaid
graph TD

ROW["Row"]

ROW --> LOCK["Lock"]

LOCK --> TX1["Transaction A"]

LOCK --> TX2["Transaction B"]
```

---

# Traditional Locking Problem

Too many locks:

```text
Blocking

Deadlocks

Reduced Throughput
```

---

# MVCC

Modern databases use:

```text
Multi-Version Concurrency Control
```

---

# MVCC Mental Model

Instead of changing rows:

```text
Create New Version
```

---

# MVCC Architecture

```mermaid
graph TD

ROW["Row"]

ROW --> V1["Version 1"]

ROW --> V2["Version 2"]

ROW --> V3["Version 3"]
```

---

# Benefits

Readers:

```text
Don't Block Writers
```

Writers:

```text
Don't Block Readers
```

---

# PostgreSQL MVCC

Each row contains:

```text
xmin

xmax
```

Tracking transaction visibility.

---

# Query Execution Flow

```mermaid
flowchart TD

QUERY["Query"]

QUERY --> PARSE["Parser"]

PARSE --> OPT["Optimizer"]

OPT --> PLAN["Plan"]

PLAN --> CACHE["Buffer Cache"]

CACHE --> STORAGE["Storage Engine"]

STORAGE --> RESULT["Result"]
```

---

# Database Filesystem Relationship

Databases ultimately use:

```text
Linux Filesystems
```

---

# Storage Stack

```mermaid
graph TD

DATABASE["Database"]

DATABASE --> FS["Filesystem"]

FS --> BLOCK["Block Layer"]

BLOCK --> DRIVER["Driver"]

DRIVER --> SSD["SSD"]
```

---

# Linux Page Cache

Interaction:

```mermaid
graph TD

DATABASE["Database"]

DATABASE --> PAGECACHE["Linux Page Cache"]

PAGECACHE --> DISK["Disk"]
```

Some databases bypass page cache.

---

# Replication

Production systems need redundancy.

---

# Replication Architecture

```mermaid
graph TD

PRIMARY["Primary"]

PRIMARY --> REPLICA1["Replica"]

PRIMARY --> REPLICA2["Replica"]
```

---

# Replication Flow

```mermaid
flowchart LR

WRITE["Write"]

WRITE --> PRIMARY["Primary"]

PRIMARY --> WAL["WAL"]

WAL --> REPLICA["Replica"]
```

---

# Why Replication Exists

```text
High Availability

Read Scaling

Disaster Recovery
```

---

# Sharding

Single databases eventually become too large.

---

# Sharding Architecture

```mermaid
graph TD

USERS["Users"]

USERS --> SHARD1["Shard A"]

USERS --> SHARD2["Shard B"]

USERS --> SHARD3["Shard C"]
```

---

# Example

```text
User 1-1M → Shard A

User 1M-2M → Shard B

User 2M-3M → Shard C
```

---

# Distributed Databases

Multiple database nodes work together.

---

# Distributed Architecture

```mermaid
graph TD

NODE1["Node A"]

NODE2["Node B"]

NODE3["Node C"]

NODE1 --> NODE2

NODE2 --> NODE3
```

---

# New Problems Appear

```text
Network Failures

Consistency

Leader Election

Replication Lag
```

---

# CAP Theorem

Distributed databases must balance:

```mermaid
graph TD

CAP["CAP"]

CAP --> C["Consistency"]

CAP --> A["Availability"]

CAP --> P["Partition Tolerance"]
```

---

# Observability

Database health requires visibility.

---

# Metrics

```text
Query Latency

Connections

Cache Hit Ratio

Replication Lag

Transactions
```

---

# Observability Architecture

```mermaid
graph TD

DATABASE["Database"]

DATABASE --> METRICS["Metrics"]

DATABASE --> LOGS["Logs"]

DATABASE --> TRACES["Traces"]
```

---

# Common Production Bottlenecks

```text
Missing Indexes

Slow Queries

Lock Contention

Replication Lag

Disk Saturation

Connection Exhaustion
```

---

# Slow Query Example

Without index:

```sql
SELECT * FROM users WHERE email='vip@example.com';
```

Result:

```text
Full Table Scan
```

---

# With Index

```sql
CREATE INDEX idx_email
ON users(email);
```

Result:

```text
Index Lookup
```

---

# Database Troubleshooting Workflow

```mermaid
flowchart TD

SLOW["Slow Database"]

SLOW --> QUERY["Check Queries"]

QUERY --> INDEX["Check Indexes"]

INDEX --> CACHE["Check Cache"]

CACHE --> STORAGE["Check Disk"]

STORAGE --> FIX["Optimize"]
```

---

# Complete Database Internals Map

```mermaid
mindmap
  root((Database))

    Query Engine
      Parser
      Optimizer
      Executor

    Storage
      Pages
      Files

    Indexes
      B Trees

    Memory
      Buffer Cache

    Recovery
      WAL

    Transactions
      ACID
      MVCC

    Scaling
      Replication
      Sharding

    Distributed Systems
      CAP
      Consensus

    Observability
      Metrics
      Logs
```

---

# Engineering Mindset

Beginners see:

```text
SELECT *
```

Engineers see:

```text
Parser
   ↓
Optimizer
   ↓
Execution Plan
   ↓
Indexes
   ↓
Buffer Cache
   ↓
Storage Engine
   ↓
Filesystem
   ↓
Linux Kernel
   ↓
Disk
```

Every query is a journey through multiple subsystems.

---

# Interview Questions

### What happens when a SQL query is executed?

### What is a query optimizer?

### Why are indexes important?

### What is a B-Tree?

### What is WAL?

### Why does WAL exist?

### What is ACID?

### What is MVCC?

### Why does PostgreSQL use MVCC?

### What is replication?

### What is sharding?

### What is the CAP theorem?

### Why are databases memory-intensive?

### What causes slow queries?

### How do databases interact with Linux?

---

# One-Page Architecture Summary

```text
SQL Query
      ↓
Parser
      ↓
Optimizer
      ↓
Execution Plan
      ↓
Indexes
      ↓
Buffer Cache
      ↓
Storage Engine
      ↓
Filesystem
      ↓
Linux Kernel
      ↓
Disk
```

---

# Final Takeaway

A database is not just a place to store data.

It is a sophisticated execution engine built from:

```text
Query Processing

Indexes

Caching

Transactions

Recovery

Concurrency Control

Replication

Storage Engines
```

Every modern application, cloud platform, Kubernetes cluster, and distributed system ultimately depends on these database foundations.

Master database internals and you gain the ability to design scalable systems, diagnose production bottlenecks, optimize performance, and understand how data truly moves from memory to disk and across the world.
