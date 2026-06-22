
# Cloud Linux Interview Questions

> Goal: Learn how engineers think, not what engineers memorize.

---

# How To Use This File

Do NOT memorize answers.

For every question ask:

```text
WHY?

HOW?

WHAT PROBLEM DOES IT SOLVE?

WHAT HAPPENS IF IT FAILS?

HOW DOES IT SCALE?
```

If you can answer these, you understand the topic.

---

# Level 1: Beginner Linux + Cloud Foundations

## Q1

What is cloud computing?

---

## Q2

Why did cloud computing become popular?

---

## Q3

What problems does cloud solve?

---

## Q4

Explain On-Premise vs Cloud.

---

## Q5

What is IaaS?

---

## Q6

What is PaaS?

---

## Q7

What is SaaS?

---

## Q8

Why does cloud heavily depend on Linux?

---

## Q9

Where does Linux exist inside AWS?

---

## Q10

Where does Linux exist inside Azure?

---

## Q11

Where does Linux exist inside GCP?

---

# Level 2: Virtualization Fundamentals

## Q12

What is virtualization?

---

## Q13

Why do virtual machines exist?

---

## Q14

What is a hypervisor?

---

## Q15

Explain Type 1 hypervisors.

---

## Q16

Explain Type 2 hypervisors.

---

## Q17

Why are VMs important for cloud computing?

---

## Q18

Explain CPU virtualization.

---

## Q19

Explain memory virtualization.

---

## Q20

Explain storage virtualization.

---

## Q21

Explain network virtualization.

---

## Q22

Why did virtualization change computing forever?

---

# Level 3: Cloud Instances

## Q23

What is a cloud instance?

---

## Q24

Explain the cloud instance lifecycle.

---

## Q25

What is cloud-init?

---

## Q26

Why are instances considered disposable?

---

## Q27

What is immutable infrastructure?

---

## Q28

Why should engineers avoid SSHing into production?

---

## Q29

Explain Pets vs Cattle.

---

## Q30

How do cloud instances scale?

---

# Level 4: Networking Fundamentals

## Q31

What is a VPC?

---

## Q32

Why do VPCs exist?

---

## Q33

Why is cloud networking software defined networking?

---

## Q34

What is a subnet?

---

## Q35

Why do subnets exist?

---

## Q36

What makes a subnet public?

---

## Q37

What makes a subnet private?

---

## Q38

What is an Internet Gateway?

---

## Q39

What is a NAT Gateway?

---

## Q40

What problem does NAT solve?

---

## Q41

Explain route tables.

---

## Q42

What is CIDR?

---

## Q43

Explain traffic flow from a user to a Linux application.

---

## Q44

Why should databases remain private?

---

# Level 5: Load Balancing

## Q45

Why do load balancers exist?

---

## Q46

What problem do they solve?

---

## Q47

Explain Round Robin.

---

## Q48

Explain Least Connections.

---

## Q49

Explain health checks.

---

## Q50

What is sticky session?

---

## Q51

Why are stateless systems easier to scale?

---

## Q52

Explain Layer 4 vs Layer 7 load balancing.

---

# Level 6: Storage Systems

## Q53

Explain block storage from first principles.

---

## Q54

Explain object storage from first principles.

---

## Q55

Explain file storage from first principles.

---

## Q56

When should you use block storage?

---

## Q57

When should you use object storage?

---

## Q58

When should you use file storage?

---

## Q59

Explain IOPS.

---

## Q60

Explain durability.

---

## Q61

Explain storage replication.

---

## Q62

Why shouldn't videos be stored inside databases?

---

# Level 7: IAM And Security

## Q63

What is IAM?

---

## Q64

What is authentication?

---

## Q65

What is authorization?

---

## Q66

What is RBAC?

---

## Q67

What is ABAC?

---

## Q68

What is Zero Trust?

---

## Q69

What is the Principle Of Least Privilege?

---

## Q70

Why are service accounts important?

---

## Q71

Why should machines have identities?

---

# Level 8: Autoscaling

## Q72

What is autoscaling?

---

## Q73

Why does autoscaling exist?

---

## Q74

What metrics drive autoscaling?

---

## Q75

Explain reactive scaling.

---

## Q76

Explain predictive scaling.

---

## Q77

Explain scheduled scaling.

---

## Q78

Why do stateless applications help autoscaling?

---

## Q79

What is a cold start problem?

---

# Level 9: Cloud Architecture Patterns

## Q80

What is three-tier architecture?

---

## Q81

What is cache-aside?

---

## Q82

What is CQRS?

---

## Q83

What is event driven architecture?

---

## Q84

What is a queue based architecture?

---

## Q85

What is immutable infrastructure?

---

## Q86

What is microservices architecture?

---

## Q87

What is an API gateway?

---

## Q88

What is a circuit breaker?

---

## Q89

What is a bulkhead pattern?

---

# Level 10: Linux Relationships

## Q90

Why does cloud not replace Linux?

---

## Q91

Explain Linux networking inside cloud.

---

## Q92

Explain Linux storage inside cloud.

---

## Q93

Explain Linux security inside cloud.

---

## Q94

Explain Linux observability inside cloud.

---

## Q95

Explain Linux boot process inside cloud instances.

---

# Level 11: Kubernetes Relationships

## Q96

Why does Kubernetes fit naturally into cloud infrastructure?

---

## Q97

How does Kubernetes use VPCs?

---

## Q98

How does Kubernetes use Linux?

---

## Q99

How does Kubernetes use storage?

---

## Q100

How does Kubernetes use load balancing?

---

## Q101

How does Kubernetes use autoscaling?

---

# Level 12: Production Engineering

## Q102

Design infrastructure for:

```text
1000 users
```

---

## Q103

Design infrastructure for:

```text
100,000 users
```

---

## Q104

Design infrastructure for:

```text
10 million users
```

---

## Q105

How would you deploy a MERN application in production?

---

## Q106

How would you deploy a Python AI application?

---

## Q107

How would you build a highly available system?

---

## Q108

How would you build a fault tolerant system?

---

## Q109

How would you build a globally distributed system?

---

# Level 13: Troubleshooting Questions

## Q110

Users cannot access your application.

Where do you start?

---

## Q111

The application is slow.

How do you debug it?

---

## Q112

Linux cannot access the internet.

How do you debug it?

---

## Q113

Database latency suddenly increased.

What do you inspect?

---

## Q114

Cloud costs suddenly doubled.

What do you inspect?

---

## Q115

Kubernetes pods are crashing.

What do you inspect?

---

# Level 14: SRE Thinking

## Q116

What does reliability mean?

---

## Q117

What does availability mean?

---

## Q118

What does durability mean?

---

## Q119

Explain SLI.

---

## Q120

Explain SLO.

---

## Q121

Explain SLA.

---

## Q122

Why does observability matter?

---

## Q123

Explain Logs vs Metrics vs Traces.

---

# Level 15: Founder Thinking

## Q124

Would you optimize for performance or cost?

---

## Q125

When should a startup adopt Kubernetes?

---

## Q126

When should a startup adopt microservices?

---

## Q127

How can infrastructure bankrupt a startup?

---

## Q128

What infrastructure choices should an early-stage startup avoid?

---

## Q129

What infrastructure decisions create technical debt?

---

## Q130

What infrastructure decisions compound positively over time?

---

# Engineering Whiteboard Questions

## Draw these architectures from memory.

### Architecture 1

```text
Users

↓

Load Balancer

↓

Linux

↓

Database
```

---

### Architecture 2

```text
Users

↓

CDN

↓

Load Balancer

↓

Linux

↓

Redis

↓

Database

↓

Object Storage
```

---

### Architecture 3

```text
Users

↓

Global Load Balancer

↓

Regional Systems

↓

Kubernetes

↓

Databases
```

---

# Ultimate Cloud Engineering Questions

If all cloud providers disappeared tomorrow:

Can you still explain?

```text
Virtualization

Linux

Networking

Storage

Security

Distributed Systems
```

If yes.

You understand cloud.

Because cloud is mostly:

```text
Linux

+

Networking

+

Storage

+

Security

+

Automation

+

Distributed Systems
```

at data center scale.

---

# Final Thought

Memorizers ask:

> What service should I use?

Engineers ask:

> What problem am I solving?

Architects ask:

> How will this scale?

Founders ask:

> Is this sustainable for the business?

The goal of this repository is to help readers gradually evolve through all four ways of thinking.
