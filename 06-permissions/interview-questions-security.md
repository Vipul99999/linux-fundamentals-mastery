# Linux Permissions Security Interview Questions

> Security-focused Linux permission questions for Security Engineers, SOC Analysts, Incident Responders, Red Teamers, Blue Teamers, Security Architects, DevSecOps Engineers, and Platform Security Engineers.

---

# Section 1

# Security Fundamentals

---

## Q1. What is the Principle of Least Privilege?

### Answer

Grant only the minimum permissions required to perform a task.

Example:

```text
Bad:
Application runs as root

Good:
Application runs as dedicated service account
```

---

## Q2. Why is Least Privilege important?

### Answer

Reduces:

```text
Attack Surface

Privilege Escalation

Accidental Damage

Lateral Movement
```

---

## Q3. What is Defense in Depth?

### Answer

Multiple independent security layers.

Visualization:

```text
Application
      │
      ▼
Permissions
      │
      ▼
ACL
      │
      ▼
Capabilities
      │
      ▼
SELinux/AppArmor
      │
      ▼
Monitoring
```

---

## Q4. Why are Linux permissions considered a security boundary?

### Answer

They restrict access to:

```text
Files

Directories

Devices

Sockets

Processes
```

---

## Q5. What is the difference between Authentication and Authorization?

### Answer

Authentication:

```text
Who are you?
```

Authorization:

```text
What can you do?
```

Permissions belong to:

```text
Authorization
```

---

# Section 2

# Privilege Escalation

---

## Q6. What is Privilege Escalation?

### Answer

A user gains permissions beyond intended access.

---

## Q7. What is Vertical Privilege Escalation?

### Answer

Example:

```text
User
  ↓
Root
```

---

## Q8. What is Horizontal Privilege Escalation?

### Answer

Example:

```text
User A
   ↓
User B
```

---

## Q9. Why are SUID binaries attractive targets?

### Answer

They execute with elevated privileges.

Vulnerabilities may lead to:

```text
Root Access
```

---

## Q10. How would you audit SUID binaries?

### Answer

```bash
find / -perm -4000 -type f
```

---

## Q11. What common vulnerabilities affect SUID programs?

### Answer

```text
Buffer Overflows

Command Injection

Path Hijacking

Race Conditions

Environment Variable Abuse
```

---

## Q12. Why should unused SUID binaries be removed?

### Answer

Reduce privilege escalation opportunities.

---

# Section 3

# File Permission Security

---

## Q13. Why is chmod 777 dangerous?

### Answer

Allows everyone:

```text
Read

Write

Execute
```

Potential consequences:

```text
Tampering

Malware Placement

Data Theft
```

---

## Q14. Why are world-writable files dangerous?

### Answer

Attackers may:

```text
Modify Files

Replace Scripts

Inject Malicious Content
```

---

## Q15. How do you locate world-writable files?

### Answer

```bash
find / -perm -002
```

---

## Q16. Why should SSH private keys use 600 permissions?

### Answer

Prevent unauthorized access.

---

## Q17. What permissions should password files have?

### Answer

Sensitive files should be highly restricted.

Typical examples:

```text
600

640
```

depending on system design.

---

# Section 4

# Ownership Security

---

## Q18. Why is file ownership important?

### Answer

Ownership determines who controls access.

---

## Q19. What security issue occurs when application files are owned by users?

### Answer

Developers may modify production code.

---

## Q20. Why use dedicated service accounts?

### Answer

Provides:

```text
Isolation

Auditing

Least Privilege
```

---

## Q21. What happens if a web server runs as root?

### Answer

Compromise may become:

```text
Full System Compromise
```

---

# Section 5

# ACL Security

---

## Q22. What security risk do ACLs introduce?

### Answer

Hidden permissions.

Administrators may overlook access grants.

---

## Q23. Why must ACLs be audited?

### Answer

Users may retain access after role changes.

---

## Q24. How do you audit ACLs?

### Answer

```bash
getfacl
```

---

## Q25. What is ACL permission creep?

### Answer

ACL entries accumulate over time.

Result:

```text
Excessive Access
```

---

# Section 6

# Linux Capabilities Security

---

## Q26. Why are capabilities safer than SUID?

### Answer

Grant specific privileges instead of full root privileges.

---

## Q27. What capability allows network administration?

### Answer

```text
CAP_NET_ADMIN
```

---

## Q28. Why is CAP_SYS_ADMIN dangerous?

### Answer

Huge privilege scope.

Often called:

```text
The New Root
```

---

## Q29. Why audit capabilities?

### Answer

Misconfigured capabilities can enable privilege escalation.

---

## Q30. How do you audit capabilities?

### Answer

```bash
getcap -r /
```

---

# Section 7

# SELinux Security

---

## Q31. What security problem does SELinux solve?

### Answer

Limits damage even when permissions are misconfigured.

---

## Q32. Why is SELinux considered Defense in Depth?

### Answer

Provides another layer beyond:

```text
Ownership

Permissions

ACLs
```

---

## Q33. Why should SELinux not be disabled?

### Answer

Removes an important protection layer.

---

## Q34. What is Type Enforcement?

### Answer

Restricts process-to-object interactions.

---

## Q35. What command investigates SELinux denials?

### Answer

```bash
ausearch -m avc
```

---

## Q36. What is the most common SELinux problem?

### Answer

Incorrect labels.

---

# Section 8

# AppArmor Security

---

## Q37. Why is AppArmor useful?

### Answer

Restricts application behavior.

---

## Q38. What is AppArmor complain mode?

### Answer

Violations logged but not blocked.

---

## Q39. What is AppArmor enforce mode?

### Answer

Violations blocked and logged.

---

## Q40. Why should AppArmor profiles be reviewed?

### Answer

Profiles may become outdated.

---

# Section 9

# Container Security

---

## Q41. Why should containers avoid root?

### Answer

Limits impact of compromise.

---

## Q42. Why are privileged containers dangerous?

### Answer

They gain extensive host access.

---

## Q43. What Linux features secure containers?

### Answer

```text
Namespaces

cgroups

Capabilities

SELinux

AppArmor
```

---

## Q44. Why drop unnecessary capabilities?

### Answer

Reduce attack surface.

---

## Q45. What causes many container permission issues?

### Answer

UID/GID mismatches.

---

# Section 10

# Auditing & Compliance

---

## Q46. What is permission auditing?

### Answer

Reviewing access rights for security risks.

---

## Q47. What should be audited regularly?

### Answer

```text
SUID

SGID

ACLs

Capabilities

World-Writable Files

Service Accounts
```

---

## Q48. Why audit dormant accounts?

### Answer

Unused accounts become attack targets.

---

## Q49. What is permission drift?

### Answer

Permissions gradually diverge from approved configuration.

---

## Q50. How can permission drift be prevented?

### Answer

```text
Automation

Infrastructure as Code

Regular Audits
```

---

# Section 11

# Incident Response

---

## Q51. A malicious script appears in /tmp. Why is this concerning?

### Answer

Shared writable directory.

Potential attacker activity.

---

## Q52. Why is /tmp protected with Sticky Bit?

### Answer

Prevents users deleting others' files.

---

## Q53. What permission indicators suggest compromise?

### Answer

```text
Unexpected SUID

New Capabilities

World-Writable Sensitive Files

Modified Ownership
```

---

## Q54. What would you investigate after finding a suspicious SUID binary?

### Answer

```text
Origin

Purpose

Owner

Creation Time

Usage
```

---

## Q55. Why review file ownership during investigations?

### Answer

Ownership changes may indicate attacker activity.

---

# Section 12

# Security Architecture Questions

---

## Q56. Design a secure web server.

Expected Concepts:

```text
Dedicated Service Account

Restricted Permissions

SELinux

Read-Only Configurations

Logging
```

---

## Q57. Design secure shared storage.

Expected Concepts:

```text
Groups

ACLs

SGID

Sticky Bit

Auditing
```

---

## Q58. Design secure container platform.

Expected Concepts:

```text
Non-Root Containers

Capability Reduction

SELinux

AppArmor

Read-Only Filesystems
```

---

## Q59. How would you secure secrets stored on Linux?

Expected Concepts:

```text
Restricted Ownership

600 Permissions

Encryption

Auditing
```

---

## Q60. How would you implement Defense in Depth?

Expected Layers:

```text
Permissions
      │
      ▼
ACL
      │
      ▼
Capabilities
      │
      ▼
SELinux
      │
      ▼
Monitoring
```

---

# Rapid Fire

Most dangerous capability?

```text
CAP_SYS_ADMIN
```

---

Find SUID binaries?

```bash
find / -perm -4000
```

---

Find world-writable files?

```bash
find / -perm -002
```

---

Check SELinux status?

```bash
sestatus
```

---

View capabilities?

```bash
getcap -r /
```

---

View ACLs?

```bash
getfacl
```

---

Restore SELinux labels?

```bash
restorecon
```

---

# Security Engineer Mindset

A strong security engineer asks:

```text
Who has access?

Who should have access?

How can access be abused?

How is access monitored?

How is privilege escalation prevented?
```

instead of simply asking:

```text
What permissions are set?
```

---

# Final Takeaways

1. Permissions are security controls.
2. Least Privilege is the most important principle.
3. SUID binaries require auditing.
4. Capabilities are safer than broad root access.
5. SELinux provides powerful protection.
6. AppArmor adds application confinement.
7. Containers still rely on Linux permissions.
8. Auditing prevents permission creep.
9. Incident responders must understand permissions deeply.
10. Security is about controlling and monitoring access.
