# 🚨 Linux Users & Groups — Interview Questions 4 (Production Incidents, Outages, Breaches & Forensics)

> Focus: Real-world disasters, authentication outages, LDAP failures, Active Directory incidents, sudo misconfigurations, PAM lockouts, Kubernetes RBAC mistakes, Cloud IAM breaches, forensic investigations, Red Team attack chains, and Blue Team response strategies.

---

# 🎯 Purpose

Most interview questions test:

```text
What command do you know?
```

These questions test:

```text
Can you keep production alive?
Can you investigate incidents?
Can you prevent breaches?
Can you design secure systems?
```

---

# 🔥 Section 1 — Authentication Outage Scenarios

---

## Incident #1

### Situation

Suddenly:

```text
Nobody can log into Linux servers.
```

### Questions

1. What would you check first?
2. How do you determine whether this is:

   * LDAP issue?
   * DNS issue?
   * PAM issue?
   * SSSD issue?
3. How do you restore service quickly?
4. How do you avoid making the outage worse?

---

## Incident #2

### Situation

SSH logins fail.

Console logins work.

### Questions

1. Which subsystem is likely affected?
2. What logs would you inspect?
3. Could PAM be involved?
4. Could SSH keys still work?

---

## Incident #3

### Situation

Password authentication fails.

SSH key authentication succeeds.

### Questions

1. What does this suggest?
2. Which files should be checked?
3. Could PAM be the cause?

---

# 🚨 Section 2 — Active Directory & LDAP Failures

---

## Incident #4

### Situation

AD users cannot log into Linux.

Local users work.

### Questions

1. Where would you investigate?
2. How does Linux communicate with AD?
3. What role does SSSD play?
4. What role does Kerberos play?

---

## Incident #5

### Situation

Only some AD users can login.

Others cannot.

### Questions

1. Group mapping issue?
2. PAM issue?
3. SSSD cache issue?
4. Access control policy issue?

---

## Incident #6

### Situation

Authentication suddenly becomes extremely slow.

### Questions

1. Could DNS be responsible?
2. Could LDAP replication be failing?
3. Could Kerberos delays exist?
4. How would you prove it?

---

# 🛑 Section 3 — Sudo Disasters

---

## Incident #7

### Situation

Every sudo command fails.

### Questions

1. Is sudo broken?
2. Is sudoers corrupted?
3. Is PAM involved?
4. How would you recover?

---

## Incident #8

### Situation

An administrator edited:

```text
/etc/sudoers
```

directly.

Now sudo no longer works.

### Questions

1. What happened?
2. Why is visudo safer?
3. How would you recover?

---

## Incident #9

### Situation

Production server compromised.

Attacker gained:

```text
NOPASSWD: ALL
```

### Questions

1. What is the impact?
2. What evidence should be collected?
3. What systems might now be compromised?
4. How do you determine attack scope?

---

# 🔥 Section 4 — PAM Lockout Incidents

---

## Incident #10

### Situation

All users locked out after PAM change.

### Questions

1. Why can PAM changes be dangerous?
2. How would you regain access?
3. How would you test PAM safely?

---

## Incident #11

### Situation

Users authenticate successfully.

Login still denied.

### Questions

1. Can PAM do this?
2. Authentication vs authorization?
3. What modules should be inspected?

---

# 🚨 Section 5 — Identity & Access Breaches

---

## Incident #12

### Situation

Former employee accesses production.

### Questions

1. What failed?
2. Was account disabled?
3. Was SSH access removed?
4. Were API tokens revoked?
5. Were certificates revoked?

---

## Incident #13

### Situation

Employee terminated.

Account locked.

Two days later:

```text
Access still observed.
```

### Questions

1. How is this possible?
2. Existing sessions?
3. SSH keys?
4. Cloud credentials?
5. Kubernetes tokens?

---

## Incident #14

### Situation

Contractor account expires.

Yet access continues.

### Questions

1. What identity systems might still trust them?
2. Which tokens should be checked?

---

# 🔍 Section 6 — Forensics Investigations

---

## Incident #15

### Situation

Unknown user gained root access.

### Questions

1. What logs would you examine?
2. How do you determine timeline?
3. How do you identify privilege escalation?

---

### Investigation Sources

```text
auth.log
secure
auditd
journalctl
sudo logs
shell history
```

---

## Incident #16

### Situation

Database deleted.

Nobody admits responsibility.

### Questions

1. How can logs help?
2. Why are shared accounts dangerous?
3. How would centralized logging help?

---

## Incident #17

### Situation

Root account used at 3:17 AM.

### Questions

1. Who used it?
2. How do you prove attribution?
3. Why should root login be disabled?

---

# 🧠 Section 7 — Red Team Attack Chains

---

## Scenario #18

Attacker obtains:

```text
Developer Password
```

### Questions

1. What happens next?
2. How would attacker enumerate sudo?
3. How would attacker search for escalation paths?

---

## Scenario #19

Attacker has:

```text
Low Privilege User
```

### Questions

1. How might PATH manipulation help?
2. How might writable scripts help?
3. How might cron jobs help?

---

## Scenario #20

Attacker finds:

```text
sudo vim
```

permission.

### Questions

1. Why might this be dangerous?
2. What is shell escape?
3. How could root shell be obtained?

---

## Scenario #21

Attacker finds:

```text
CAP_SYS_ADMIN
```

### Questions

1. Why is this dangerous?
2. Why is it called "new root"?

---

# 🛡️ Section 8 — Blue Team Response

---

## Scenario #22

Compromised account detected.

### Questions

1. Delete or lock?
2. Why?
3. What evidence should be preserved?

---

## Scenario #23

Privilege escalation detected.

### Questions

1. What logs would you preserve?
2. What accounts should be reviewed?
3. What sudo rules should be audited?

---

## Scenario #24

Malicious insider suspected.

### Questions

1. What evidence is needed?
2. What legal concerns exist?
3. What audit trails matter?

---

# ☁️ Section 9 — Cloud IAM Breaches

---

## Incident #25

AWS credentials leaked.

### Questions

1. Disable user?
2. Rotate keys?
3. Review CloudTrail?
4. How determine impact?

---

## Incident #26

Cloud admin account compromised.

### Questions

1. Why is this worse than normal user compromise?
2. What resources might be affected?

---

## Incident #27

Developer accidentally grants:

```text
AdministratorAccess
```

to application role.

### Questions

1. What risks exist?
2. How does least privilege help?

---

# ☸️ Section 10 — Kubernetes RBAC Incidents

---

## Incident #28

Developer accidentally receives:

```text
cluster-admin
```

### Questions

1. Impact?
2. Equivalent Linux concept?
3. Why dangerous?

---

## Incident #29

Pod gains unexpected cluster access.

### Questions

1. Service account issue?
2. RBAC issue?
3. Token exposure issue?

---

## Incident #30

Compromised pod accesses Kubernetes API.

### Questions

1. What permissions should be investigated?
2. What role bindings matter?

---

# 🔐 Section 11 — Zero Trust Scenarios

---

## Incident #31

Valid credentials used from unusual country.

### Questions

1. Should access be trusted?
2. What additional checks should occur?

---

## Incident #32

Employee authenticated successfully.

Device is infected.

### Questions

1. Should access be granted?
2. Why does Zero Trust care about device health?

---

## Incident #33

User passes MFA.

Behavior suddenly changes.

### Questions

1. What risk indicators exist?
2. What should adaptive authentication do?

---

# 🏗️ Section 12 — Architecture Design Questions

---

## Design #34

Design identity management for:

```text
100 Servers
500 Employees
Hybrid Cloud
```

Discuss:

```text
LDAP
AD
SSSD
MFA
RBAC
Audit Logs
```

---

## Design #35

Design secure privileged access.

Requirements:

```text
No shared accounts
Full auditing
Emergency access
Least privilege
```

---

## Design #36

Design break-glass accounts.

Questions:

1. When used?
2. How protected?
3. How audited?

---

# 🔥 Ultimate Staff-Level Incident

---

## Scenario #37

At 2:00 AM:

```text
LDAP fails
```

At 2:05 AM:

```text
Authentication failures begin
```

At 2:15 AM:

```text
Production outage starts
```

At 2:20 AM:

```text
Engineers lose sudo access
```

At 2:30 AM:

```text
Kubernetes deployments fail
```

At 2:45 AM:

```text
Cloud automation stops
```

### Explain:

```text
Identity dependencies
Authentication flow
Authorization flow
Failure propagation
Recovery strategy
Business impact
Security implications
Long-term fixes
```

---

# 🎯 Final Principal Engineer Challenge

Design a complete identity architecture for:

```text
50,000 Employees
10,000 Linux Servers
Multi-Cloud Environment
Kubernetes Clusters
Remote Workforce
Zero Trust Security
24x7 Operations
Compliance Requirements
```

Using:

```text
Active Directory
LDAP
Kerberos
SSSD
FreeIPA
PAM
sudo
RBAC
IAM
OIDC
SAML
Passkeys
MFA
Audit Logging
SIEM
Zero Trust
```

Then explain:

```text
How users authenticate
How services authenticate
How authorization works
How incidents are investigated
How credentials are rotated
How outages are handled
How recovery occurs
```

If you can confidently answer most questions in this file, you're thinking like a Senior/Staff/Principal Engineer rather than someone who only knows Linux commands.
