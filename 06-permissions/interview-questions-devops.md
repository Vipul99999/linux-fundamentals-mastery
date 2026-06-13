# Linux Permissions for DevOps Interviews

> This guide focuses on Linux permission questions commonly asked in DevOps, Platform Engineering, Cloud Engineering, Infrastructure Engineering, and Kubernetes interviews.

---

# Section 1
# Core DevOps Permission Questions

---

## Q1. Why are Linux permissions important in DevOps?

### Answer

Because every DevOps system relies on:

```text
Servers

Containers

Volumes

CI/CD Pipelines

Secrets

Service Accounts
```

Incorrect permissions can cause:

```text
Outages

Security Breaches

Deployment Failures
```

---

## Q2. What Linux permission topics should every DevOps engineer know?

### Answer

```text
Ownership

chmod

chown

ACLs

SUID

Capabilities

SELinux

Docker Permissions

Kubernetes Security Contexts
```

---

## Q3. Why is chmod 777 considered a bad DevOps practice?

### Answer

Because it violates:

```text
Least Privilege
```

and increases attack surface.

---

## Q4. What is the Principle of Least Privilege?

### Answer

Grant:

```text
Only Required Access
```

Nothing more.

---

## Q5. How would you secure application configuration files?

### Answer

Typical permissions:

```bash
640
```

or

```bash
600
```

Example:

```text
database.yml

.env

application.properties
```

---

# Section 2
# CI/CD Questions

---

## Q6. A Jenkins pipeline cannot execute a script.

What do you check?

### Answer

```text
Execute Permission

Ownership

Agent User

SELinux

Mount Options
```

---

## Q7. Why might a CI/CD pipeline fail after copying files?

### Answer

Permissions may not be preserved.

Common issues:

```text
Ownership Changed

Execute Bit Lost

ACL Missing
```

---

## Q8. How do you preserve permissions during deployment?

### Answer

Common tools:

```bash
rsync -a

tar

scp -p
```

---

## Q9. Why do deployment scripts often use service accounts?

### Answer

Avoid:

```text
Root Access
```

Improve:

```text
Security

Auditing
```

---

## Q10. What permission problems commonly break CI/CD pipelines?

### Answer

```text
Missing Execute Bit

Wrong Ownership

Container UID Mismatch

Secrets Access Failure
```

---

# Section 3
# Docker Questions

---

## Q11. Why do Docker containers frequently experience permission issues?

### Answer

Most common reason:

```text
UID/GID mismatch
```

between:

```text
Host

Container
```

---

## Q12. Example?

### Answer

Container:

```text
UID=1000
```

Host File:

```text
UID=2000
```

Result:

```text
Permission Denied
```

---

## Q13. Why is:

```bash
docker run --privileged
```

dangerous?

### Answer

Container gains:

```text
Nearly Full Host Access
```

---

## Q14. What is a safer alternative?

### Answer

Use:

```bash
--cap-drop ALL
```

then add only required capabilities.

---

## Q15. Why should containers avoid running as root?

### Answer

Compromised container:

```text
Less Damage

Reduced Privilege Escalation Risk
```

---

## Q16. How do you run containers as non-root?

### Answer

Dockerfile:

```dockerfile
USER appuser
```

---

## Q17. What Linux permissions still apply inside containers?

### Answer

All traditional concepts:

```text
UID

GID

Ownership

Permissions

ACLs
```

---

# Section 4
# Kubernetes Questions

---

## Q18. What Kubernetes feature controls process permissions?

### Answer

```yaml
securityContext
```

---

## Q19. Why use:

```yaml
runAsNonRoot: true
```

### Answer

Prevents container processes from running as root.

---

## Q20. Why use:

```yaml
readOnlyRootFilesystem: true
```

### Answer

Reduces attack surface.

---

## Q21. What causes Kubernetes volume permission issues?

### Answer

Usually:

```text
UID mismatch

GID mismatch

Storage Class Settings
```

---

## Q22. What does fsGroup do?

### Answer

Assigns group ownership for mounted volumes.

Example:

```yaml
fsGroup: 2000
```

---

## Q23. What is the benefit of fsGroup?

### Answer

Containers can access shared volumes without:

```bash
chmod 777
```

---

## Q24. Why should privileged pods be avoided?

### Answer

They gain:

```text
Host Access

Kernel Access

Device Access
```

---

## Q25. How do Linux capabilities relate to Kubernetes?

### Answer

Capabilities can be:

```yaml
added

dropped
```

through:

```yaml
securityContext
```

---

# Section 5
# SELinux & DevOps

---

## Q26. Why is SELinux important in enterprise DevOps?

### Answer

Provides:

```text
Mandatory Access Control
```

beyond normal permissions.

---

## Q27. What is the most common SELinux issue?

### Answer

Incorrect file context.

---

## Q28. How do you check SELinux status?

### Answer

```bash
sestatus
```

---

## Q29. How do you inspect SELinux labels?

### Answer

```bash
ls -Z
```

---

## Q30. What command restores default labels?

### Answer

```bash
restorecon
```

---

# Section 6
# Cloud Questions

---

## Q31. Difference between IAM permissions and Linux permissions?

### Answer

IAM:

```text
Cloud Resource Access
```

Linux:

```text
OS Resource Access
```

---

## Q32. Example?

### Answer

IAM:

```text
Access S3 Bucket
```

Linux:

```text
Access Local File
```

---

## Q33. Can correct IAM permissions still result in access failures?

### Answer

Yes.

Linux permissions may still deny access.

---

## Q34. Why should secrets have restrictive permissions?

### Answer

Secrets often contain:

```text
Passwords

API Keys

Certificates

Tokens
```

---

## Q35. Recommended secret permissions?

### Answer

Typically:

```bash
600
```

---

# Section 7
# Monitoring & Troubleshooting

---

## Q36. A service suddenly fails after deployment.

Where do you start?

### Answer

Check:

```text
Ownership

Permissions

Logs

SELinux

systemd
```

---

## Q37. Why can a service work manually but fail under systemd?

### Answer

Different:

```text
User

Environment

SELinux Context
```

---

## Q38. How do you identify which user a service runs as?

### Answer

```bash
systemctl cat service
```

Look for:

```ini
User=
```

---

## Q39. How do you investigate file access issues?

### Answer

Useful commands:

```bash
ls -l

namei -l

getfacl

ls -Z
```

---

## Q40. Why is auditing important?

### Answer

Detect:

```text
Privilege Escalation

Permission Drift

Misconfigurations
```

---

# Section 8
# Real Production Scenarios

---

## Q41.

A container cannot write logs.

What is your first suspicion?

### Answer

```text
UID/GID mismatch
```

---

## Q42.

A deployment works in development but fails in production.

Permissions appear identical.

What next?

### Answer

Check:

```text
SELinux

AppArmor

Storage Mount Options
```

---

## Q43.

An application needs port 80 but should not run as root.

What would you do?

### Answer

Grant:

```text
CAP_NET_BIND_SERVICE
```

---

## Q44.

Developers need shared access to project files.

What Linux feature helps?

### Answer

```text
SGID
```

---

## Q45.

Developers accidentally delete each other's files.

What feature prevents this?

### Answer

```text
Sticky Bit
```

---

# Section 9
# Advanced DevOps Questions

---

## Q46. What is permission drift?

### Answer

Permissions gradually changing over time from intended configuration.

---

## Q47. How do Infrastructure as Code tools help?

### Answer

Maintain:

```text
Consistent Permissions

Consistent Ownership

Repeatable Security
```

---

## Q48. Why should permission checks be automated?

### Answer

Reduce:

```text
Human Error

Security Gaps
```

---

## Q49. What Linux permission concepts are most important for Kubernetes?

### Answer

```text
UID

GID

Capabilities

SELinux

Volumes
```

---

## Q50. What is the biggest permission mistake in modern DevOps?

### Answer

Using:

```bash
chmod 777
```

instead of identifying the root cause.

---

# DevOps Rapid Fire

---

Find SUID files?

```bash
find / -perm -4000
```

---

View ACLs?

```bash
getfacl
```

---

Check SELinux status?

```bash
sestatus
```

---

Restore SELinux labels?

```bash
restorecon
```

---

Check capabilities?

```bash
getcap
```

---

View service user?

```bash
systemctl cat service
```

---

Check groups?

```bash
id
```

---

View directory path permissions?

```bash
namei -l
```

---

# What Interviewers Actually Want

A strong DevOps candidate can explain:

```text
Why access failed

Why containers fail

Why deployments break

Why permissions drift

How to secure services

How to troubleshoot quickly
```

rather than simply memorizing chmod values.
