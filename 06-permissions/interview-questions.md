# Linux Permissions Interview Questions

> This guide contains interview questions ranging from beginner Linux administration to advanced DevOps, Security Engineering, Site Reliability Engineering (SRE), and Platform Engineering roles.

The goal is not memorization.

The goal is understanding:

```text
Why Linux behaves this way.
```

---

# Section 1
# Beginner Questions

---

## Q1. What are Linux file permissions?

### Answer

Linux file permissions determine:

```text
Who can:

Read

Write

Execute
```

a file or directory.

Permissions are assigned to:

```text
Owner

Group

Others
```

---

## Q2. What does rwxr-xr-- mean?

### Answer

```text
Owner  = rwx

Group  = r-x

Others = r--
```

Meaning:

```text
Owner:
Read Write Execute

Group:
Read Execute

Others:
Read Only
```

---

## Q3. What do r, w, and x mean?

### Answer

For files:

```text
r = Read Content

w = Modify Content

x = Execute Program
```

---

For directories:

```text
r = List Files

w = Create/Delete Entries

x = Enter Directory
```

---

## Q4. What command changes permissions?

### Answer

```bash
chmod
```

Examples:

```bash
chmod 755 script.sh

chmod u+x script.sh
```

---

## Q5. What command changes ownership?

### Answer

```bash
chown
```

Example:

```bash
chown alice file.txt
```

---

## Q6. What command changes group ownership?

### Answer

```bash
chgrp
```

Example:

```bash
chgrp developers file.txt
```

---

## Q7. What does chmod 777 do?

### Answer

Gives:

```text
Read

Write

Execute
```

to everyone.

---

Follow-Up

Why dangerous?

Because:

```text
Anyone can modify data.
```

---

## Q8. What does chmod 755 mean?

### Answer

```text
Owner:
rwx

Group:
r-x

Others:
r-x
```

Common for:

```text
Scripts

Directories
```

---

## Q9. What does chmod 644 mean?

### Answer

```text
Owner:
rw-

Group:
r--

Others:
r--
```

Common for:

```text
Configuration Files
```

---

## Q10. What is the difference between a file and directory execute bit?

### Answer

File:

```text
Execute Program
```

Directory:

```text
Traverse Directory
```

This is a very common interview question.

---

# Section 2
# Intermediate Questions

---

## Q11. How does Linux decide which permissions apply?

### Answer

Evaluation order:

```text
Owner

Group

Others
```

Linux stops after finding a match.

---

Visualization

```text
User
 │
 ▼
Owner?
 │
 ▼
Group?
 │
 ▼
Others
```

---

## Q12. Why can a user be denied access to a file with 644 permissions?

### Answer

Possible reasons:

```text
Directory Permissions

ACL

SELinux

AppArmor
```

---

## Q13. What is umask?

### Answer

A value that removes permissions from newly created files and directories.

---

Example

```bash
umask 022
```

Files:

```text
644
```

Directories:

```text
755
```

---

## Q14. Why are new files usually not executable?

### Answer

Default file permissions start at:

```text
666
```

not:

```text
777
```

Security reason.

---

## Q15. How do you check a user's groups?

### Answer

```bash
id

groups
```

---

## Q16. What does the "+" sign mean in ls -l output?

Example:

```text
-rw-r-----+
```

### Answer

ACL exists.

---

## Q17. How do you view ACLs?

### Answer

```bash
getfacl file
```

---

## Q18. How do you create an ACL?

### Answer

```bash
setfacl
```

Example:

```bash
setfacl -m u:bob:r file.txt
```

---

## Q19. What is an ACL mask?

### Answer

Maximum permission allowed for ACL entries.

---

Example:

```text
ACL = rwx

Mask = r--

Effective = r--
```

---

## Q20. What command displays capabilities?

### Answer

```bash
getcap
```

---

# Section 3
# Advanced Questions

---

## Q21. What is SUID?

### Answer

SUID causes a program to run using:

```text
File Owner UID
```

instead of:

```text
Executing User UID
```

---

## Q22. Why does passwd use SUID?

### Answer

Needs access to:

```text
/etc/shadow
```

which normal users cannot modify.

---

## Q23. What is SGID?

### Answer

SGID can:

```text
Run with Group Privileges

or

Force Group Inheritance
```

for directories.

---

## Q24. What is Sticky Bit?

### Answer

Prevents users from deleting files owned by others in shared directories.

---

Example:

```text
/tmp
```

---

## Q25. What are Linux capabilities?

### Answer

Capabilities split root privileges into smaller privileges.

---

Example:

```text
CAP_NET_BIND_SERVICE

CAP_SYS_ADMIN

CAP_NET_RAW
```

---

## Q26. Why are capabilities safer than SUID?

### Answer

Capabilities grant:

```text
Specific Privileges
```

SUID grants:

```text
Full Owner Privileges
```

---

## Q27. What is CAP_NET_BIND_SERVICE?

### Answer

Allows binding to:

```text
Ports Below 1024
```

without root.

---

## Q28. Which capability is often called "The New Root"?

### Answer

```text
CAP_SYS_ADMIN
```

---

## Q29. How do you audit SUID binaries?

### Answer

```bash
find / -perm -4000
```

---

## Q30. How do you audit capabilities?

### Answer

```bash
getcap -r /
```

---

# Section 4
# SELinux & AppArmor

---

## Q31. What is SELinux?

### Answer

Mandatory Access Control system.

---

## Q32. Difference between DAC and MAC?

### Answer

DAC:

```text
Owner Controlled
```

MAC:

```text
Policy Controlled
```

---

## Q33. What is Type Enforcement?

### Answer

SELinux policy model controlling:

```text
Process Type

Object Type
```

interactions.

---

## Q34. What command shows SELinux contexts?

### Answer

```bash
ls -Z
```

---

## Q35. What command shows SELinux status?

### Answer

```bash
sestatus
```

---

## Q36. What are SELinux modes?

### Answer

```text
Enforcing

Permissive

Disabled
```

---

## Q37. What is AppArmor?

### Answer

Path-based Mandatory Access Control.

---

## Q38. Difference between SELinux and AppArmor?

### Answer

SELinux:

```text
Label Based
```

AppArmor:

```text
Path Based
```

---

## Q39. What command checks AppArmor status?

### Answer

```bash
aa-status
```

---

## Q40. What is AppArmor complain mode?

### Answer

Violations are:

```text
Logged

Not Blocked
```

---

# Section 5
# Troubleshooting Questions

---

## Q41. User cannot access a file. What do you check first?

### Answer

```text
File Exists

Ownership

Permissions
```

---

## Q42. A file has 777 permissions but access is denied. Why?

### Answer

Possible causes:

```text
SELinux

AppArmor

Filesystem Mount Options

ACL
```

---

## Q43. Why can a 644 file still be inaccessible?

### Answer

Directory traversal issue.

---

## Q44. How do you investigate Permission Denied errors?

### Answer

Workflow:

```text
Ownership

Permissions

Groups

ACL

Capabilities

SELinux

AppArmor
```

---

## Q45. What command helps investigate SELinux denials?

### Answer

```bash
ausearch -m avc
```

---

# Section 6
# Docker & Kubernetes Questions

---

## Q46. Why do Docker volume permission issues happen?

### Answer

UID/GID mismatch.

---

## Q47. Why is --privileged dangerous?

### Answer

Container receives nearly full host privileges.

---

## Q48. What securityContext setting is recommended?

### Answer

```yaml
runAsNonRoot: true
```

---

## Q49. Why should capabilities be dropped in containers?

### Answer

Reduce attack surface.

---

## Q50. What does CAP_NET_RAW allow?

### Answer

Raw packet creation.

Used by:

```text
ping
```

---

# Section 7
# Scenario-Based Questions

---

## Q51.

A website returns 403 Forbidden.

Permissions look correct.

What next?

### Answer

Check:

```text
Directory Permissions

SELinux Contexts

AppArmor Profiles
```

---

## Q52.

Users cannot collaborate in a shared directory.

What might be missing?

### Answer

```text
SGID
```

---

## Q53.

Users delete each other's files.

How do you fix it?

### Answer

```text
Sticky Bit
```

---

## Q54.

Application cannot bind to port 80 without root.

How would you fix it?

### Answer

Use:

```text
CAP_NET_BIND_SERVICE
```

---

## Q55.

A container cannot write to a mounted volume.

What is the most common cause?

### Answer

UID mismatch.

---

# Section 8
# Expert-Level Questions

---

## Q56.

Explain Linux permission evaluation order.

### Answer

```text
Owner

Group

Others

ACL

Capabilities

SELinux/AppArmor
```

---

## Q57.

How does Linux store ACLs?

### Answer

Extended Attributes (xattr).

---

## Q58.

What is an Access Vector Cache (AVC)?

### Answer

SELinux decision cache.

---

## Q59.

Why should SUID binaries be minimized?

### Answer

Privilege escalation risk.

---

## Q60.

Design a secure shared project directory.

### Answer

Use:

```text
Groups

SGID

Sticky Bit

ACL

SELinux
```

---

# Rapid-Fire Round

---

What command changes permissions?

```bash
chmod
```

---

Changes ownership?

```bash
chown
```

---

Shows ACL?

```bash
getfacl
```

---

Shows capabilities?

```bash
getcap
```

---

Shows SELinux context?

```bash
ls -Z
```

---

Shows AppArmor status?

```bash
aa-status
```

---

Find SUID files?

```bash
find / -perm -4000
```

---

Find SGID files?

```bash
find / -perm -2000
```

---

Find Sticky Bit directories?

```bash
find / -perm -1000
```

---

# Visual Revision Map

```text
Ownership
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
Access Decision
```

---

# Key Takeaways

1. Linux permissions are foundational interview topics.

2. Ownership evaluation happens before permissions.

3. Directory permissions are frequently tested.

4. ACLs are common enterprise topics.

5. SUID/SGID questions appear in security interviews.

6. SELinux is heavily tested in Red Hat environments.

7. Docker permission issues are common DevOps questions.

8. Troubleshooting methodology matters more than memorization.

9. Scenario-based questions are increasingly common.

10. Understanding *why* Linux behaves a certain way is more valuable than memorizing commands.
