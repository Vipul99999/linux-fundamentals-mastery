# Linux Permissions Red Hat (RHCSA/RHCE) Interview Questions

> Enterprise Linux permission questions focused on Red Hat Enterprise Linux (RHEL), RHCSA, RHCE, system administration, SELinux, ACLs, troubleshooting, and production environments.

---

# Section 1

# RHCSA Fundamentals

---

## Q1. What are Linux permissions?

### Answer

Linux permissions control:

```text
Read

Write

Execute
```

for:

```text
Owner

Group

Others
```

---

## Q2. How do you view file permissions?

### Answer

```bash
ls -l
```

Example:

```text
-rwxr-xr--
```

---

## Q3. What command changes permissions?

### Answer

```bash
chmod
```

---

## Q4. What command changes ownership?

### Answer

```bash
chown
```

---

## Q5. What command changes group ownership?

### Answer

```bash
chgrp
```

---

## Q6. What does permission 755 mean?

### Answer

```text
Owner  = rwx

Group  = r-x

Others = r-x
```

---

## Q7. What does permission 644 mean?

### Answer

```text
Owner  = rw-

Group  = r--

Others = r--
```

---

## Q8. What is umask?

### Answer

Determines default permissions for new files and directories.

---

## Q9. How do you check current umask?

### Answer

```bash
umask
```

---

## Q10. What is the default permission base for files?

### Answer

```text
666
```

before umask is applied.

---

# Section 2

# ACL Administration

---

## Q11. Why use ACLs?

### Answer

Traditional permissions support:

```text
Owner

Group

Others
```

ACLs support:

```text
Multiple Users

Multiple Groups
```

---

## Q12. How do you view ACLs?

### Answer

```bash
getfacl file
```

---

## Q13. How do you grant ACL access?

### Answer

```bash
setfacl -m u:bob:rwx file
```

---

## Q14. How do you remove ACL entries?

### Answer

```bash
setfacl -x u:bob file
```

---

## Q15. How do you remove all ACLs?

### Answer

```bash
setfacl -b file
```

---

## Q16. What indicates ACL presence in ls output?

### Answer

```text
+
```

Example:

```text
-rw-r-----+
```

---

## Q17. What is a default ACL?

### Answer

ACL inherited by newly created files.

---

## Q18. How do you create a default ACL?

### Answer

```bash
setfacl -d
```

---

## Q19. What is ACL Mask?

### Answer

Maximum effective permissions for ACL entries.

---

## Q20. Why do ACL permissions sometimes appear ignored?

### Answer

ACL Mask may restrict them.

---

# Section 3

# SELinux Fundamentals

---

## Q21. What is SELinux?

### Answer

Mandatory Access Control (MAC) framework.

---

## Q22. Why is SELinux important in RHEL?

### Answer

Core enterprise security mechanism.

---

## Q23. What are SELinux modes?

### Answer

```text
Enforcing

Permissive

Disabled
```

---

## Q24. How do you check SELinux status?

### Answer

```bash
sestatus
```

---

## Q25. How do you check current enforcement mode?

### Answer

```bash
getenforce
```

---

## Q26. How do you temporarily switch to permissive mode?

### Answer

```bash
setenforce 0
```

---

## Q27. How do you return to enforcing mode?

### Answer

```bash
setenforce 1
```

---

## Q28. Why should SELinux not be permanently disabled?

### Answer

Removes an important security layer.

---

## Q29. How do you view SELinux contexts?

### Answer

```bash
ls -Z
```

---

## Q30. Example SELinux context?

### Answer

```text
system_u:object_r:httpd_sys_content_t:s0
```

---

# Section 4

# SELinux Troubleshooting

---

## Q31. Website returns 403.

Permissions look correct.

What next?

### Answer

Check SELinux labels.

---

## Q32. What command restores default labels?

### Answer

```bash
restorecon
```

---

## Q33. How do you recursively restore labels?

### Answer

```bash
restorecon -Rv /path
```

---

## Q34. How do you investigate SELinux denials?

### Answer

```bash
ausearch -m avc
```

---

## Q35. What is AVC?

### Answer

Access Vector Cache.

Stores SELinux decisions.

---

## Q36. How do you view SELinux file contexts?

### Answer

```bash
ls -Z
```

---

## Q37. Why might Apache fail to access files?

### Answer

Incorrect:

```text
SELinux Context
```

---

## Q38. What SELinux type is commonly used for web content?

### Answer

```text
httpd_sys_content_t
```

---

## Q39. What command manages persistent file context rules?

### Answer

```bash
semanage fcontext
```

---

## Q40. What is Type Enforcement?

### Answer

SELinux's primary security mechanism.

---

# Section 5

# RHCSA Hands-On Scenarios

---

## Q41.

Create a directory where:

```text
Developers can collaborate

Files inherit group ownership
```

### Answer

Use:

```bash
chmod 2775 project
```

SGID enabled.

---

## Q42.

Create a shared directory where users cannot delete each other's files.

### Answer

Use:

```bash
chmod 1777 shared
```

Sticky Bit.

---

## Q43.

Grant user bob access without changing ownership.

### Answer

Use ACL:

```bash
setfacl -m u:bob:rwx file
```

---

## Q44.

Secure SSH private key.

### Answer

```bash
chmod 600 id_rsa
```

---

## Q45.

Allow web server to read files in custom location.

### Answer

Fix:

```text
Permissions

Ownership

SELinux Context
```

---

# Section 6

# RHCE-Level Questions

---

## Q46. Why is SELinux preferred over disabling security controls?

### Answer

Provides:

```text
Defense in Depth
```

---

## Q47. What is the difference between DAC and MAC?

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

## Q48. Why does RHEL heavily emphasize SELinux?

### Answer

Enterprise security requirement.

---

## Q49. How do you persist SELinux labeling for custom web content?

### Answer

```bash
semanage fcontext
restorecon
```

---

## Q50. How would you troubleshoot a permission-denied application?

### Answer

Workflow:

```text
Ownership

Permissions

ACL

SELinux

Application Logs
```

---

# Section 7

# Enterprise Administration

---

## Q51. Why should services run under dedicated accounts?

### Answer

Limits impact of compromise.

---

## Q52. What permissions should configuration files typically use?

### Answer

Often:

```text
640

600
```

---

## Q53. Why audit SUID binaries?

### Answer

Potential privilege escalation vectors.

---

## Q54. How do you find SUID binaries?

### Answer

```bash
find / -perm -4000
```

---

## Q55. How do you find SGID binaries?

### Answer

```bash
find / -perm -2000
```

---

## Q56. How do you find world-writable files?

### Answer

```bash
find / -perm -002
```

---

## Q57. Why should world-writable files be minimized?

### Answer

Security risk.

---

## Q58. How do you audit ACLs?

### Answer

```bash
getfacl
```

---

## Q59. Why are ACLs common in enterprise environments?

### Answer

Flexible delegation of access.

---

## Q60. What is permission drift?

### Answer

Permissions gradually diverging from intended configuration.

---

# Section 8

# Troubleshooting Scenarios

---

## Q61.

User cannot access file.

Permissions appear correct.

What do you check?

### Answer

```text
Directory Permissions

ACL

SELinux

Group Membership
```

---

## Q62.

A service works manually but fails through systemd.

What do you investigate?

### Answer

```text
User

Environment

SELinux
```

---

## Q63.

A file has 777 permissions but access is denied.

Possible causes?

### Answer

```text
SELinux

Read-Only Mount

ACL

AppArmor
```

---

## Q64.

A website works after setenforce 0.

What does this suggest?

### Answer

SELinux policy or labeling issue.

---

## Q65.

Users lose access after changing groups.

What command refreshes group membership?

### Answer

User must:

```bash
newgrp
```

or re-login.

---

# RHCSA Rapid Fire

---

View permissions?

```bash
ls -l
```

---

Change permissions?

```bash
chmod
```

---

Change owner?

```bash
chown
```

---

View ACL?

```bash
getfacl
```

---

Set ACL?

```bash
setfacl
```

---

SELinux status?

```bash
sestatus
```

---

Current mode?

```bash
getenforce
```

---

Restore labels?

```bash
restorecon
```

---

Investigate AVC?

```bash
ausearch -m avc
```

---

Find SUID files?

```bash
find / -perm -4000
```

---

# RHCSA/RHCE Interview Tip

Red Hat interviews often focus less on:

```text
Theory
```

and more on:

```text
Hands-On Tasks

Troubleshooting

SELinux

ACLs

Real Administration
```

If you can diagnose:

```text
Permission Denied
```

systematically using:

```text
Ownership

Permissions

ACL

SELinux
```

you are already operating at a strong RHCSA/RHCE level.
