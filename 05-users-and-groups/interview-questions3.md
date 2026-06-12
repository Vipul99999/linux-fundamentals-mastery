# 🚀 Linux Users & Groups — Interview Questions 3 (Expert → Staff → Principal Engineer)

> Focus: Kernel Identity, Linux Capabilities, SELinux, AppArmor, Kerberos, FreeIPA, SSSD, OAuth2, OIDC, SAML, Passkeys, Cloud Federation, Advanced Security Architecture, and Real-World Incident Response.

---

# 🎯 How To Think

Don't answer:

```text
What command?
```

Answer:

```text
Why?
How?
What if it fails?
What are the risks?
How would it scale?
```

---

# 🔴 Section 1 — Linux Kernel Identity Internals

---

## Q1.

Does Linux trust usernames?

### Correct Answer

No.

Linux trusts:

```text
UID
GID
```

---

Visual

```text
Username
    │
    ▼
Lookup
    │
    ▼
UID
    │
    ▼
Kernel Trusts UID
```

---

## Q2.

Why does Linux use UID instead of usernames internally?

### Reasons

```text
Performance
Consistency
Uniqueness
Kernel Simplicity
```

---

## Q3.

Can multiple usernames map to the same UID?

### Answer

Yes.

What are the security implications?

---

## Q4.

Why is UID 0 special?

---

### Deep Concept

Kernel checks:

```text
UID == 0
```

not:

```text
Username == root
```

---

## Interview Trap

Can a user named:

```text
rahul
```

have root privileges?

Yes.

If:

```text
UID = 0
```

---

## Q5.

How does the kernel determine file ownership?

---

Stores:

```text
UID
GID
```

inside filesystem metadata.

Not usernames.

---

# 🟣 Section 2 — Linux Capabilities

---

## Q6.

Why was root considered problematic?

---

Traditional model:

```text
All Power
or
No Power
```

---

Visual

```text
root
 │
 ▼
Everything

User
 │
 ▼
Limited
```

---

## Q7.

What are Linux Capabilities?

---

Break root privileges into smaller pieces.

Example:

```text
CAP_NET_ADMIN
CAP_SYS_ADMIN
CAP_SYS_TIME
CAP_NET_BIND_SERVICE
```

---

## Q8.

Why are capabilities more secure than full root?

---

Principle:

```text
Least Privilege
```

---

## Q9.

What capability allows binding to ports below 1024?

---

Answer:

```text
CAP_NET_BIND_SERVICE
```

---

## Q10.

Why is:

```text
CAP_SYS_ADMIN
```

often called:

```text
"The New Root"
```

?

---

# 🔵 Section 3 — SELinux

---

## Q11.

What problem does SELinux solve?

---

Traditional Linux:

```text
User
Group
Permissions
```

sometimes insufficient.

---

SELinux adds:

```text
Mandatory Access Control
```

---

## Q12.

Difference between:

```text
DAC
```

and

```text
MAC
```

?

---

DAC:

```text
Discretionary Access Control
```

User controls permissions.

---

MAC:

```text
Mandatory Access Control
```

Policy controls permissions.

---

## Q13.

Can root be denied access by SELinux?

---

Yes.

This surprises many engineers.

---

## Q14.

Why does SELinux often break applications?

---

Usually because:

```text
Policy
```

does not allow operation.

---

## Q15.

Production outage:

```text
Application works after:
setenforce 0
```

What does that suggest?

---

# 🟢 Section 4 — AppArmor

---

## Q16.

Compare:

```text
SELinux
```

vs

```text
AppArmor
```

---

## Q17.

Why do Ubuntu systems often prefer AppArmor?

---

## Q18.

Which is easier to learn?

Why?

---

## Q19.

Which provides finer-grained controls?

---

## Q20.

Would you choose SELinux or AppArmor for:

```text
Bank
Government
Healthcare
```

and why?

---

# 🟡 Section 5 — Kerberos

---

## Q21.

What problem does Kerberos solve?

---

Avoid repeatedly sending passwords.

---

## Q22.

What is a Kerberos Ticket?

---

## Q23.

What is a TGT?

---

```text
Ticket Granting Ticket
```

---

## Q24.

Why is time synchronization critical in Kerberos?

---

Hint:

```text
Replay Attacks
```

---

## Q25.

What happens if clocks drift significantly?

---

# 🟠 Section 6 — SSSD & FreeIPA

---

## Q26.

What is SSSD?

---

```text
System Security Services Daemon
```

---

## Q27.

Why is SSSD important?

---

Provides:

```text
Caching
Offline Authentication
Identity Integration
```

---

## Q28.

What happens if LDAP fails but SSSD cache exists?

---

## Q29.

What is FreeIPA?

---

Linux-native identity platform.

Combines:

```text
LDAP
Kerberos
Certificates
Policies
DNS
```

---

## Q30.

When would you choose FreeIPA over Active Directory?

---

# 🔴 Section 7 — OAuth2

---

## Q31.

What problem does OAuth2 solve?

---

Delegated authorization.

---

## Q32.

Why is OAuth2 NOT authentication?

---

Common interview trap.

---

OAuth2 answers:

```text
What can access what?
```

Not:

```text
Who are you?
```

---

## Q33.

What is an Access Token?

---

## Q34.

Why are short-lived tokens safer?

---

## Q35.

What risks exist if access tokens are stolen?

---

# 🔵 Section 8 — OpenID Connect (OIDC)

---

## Q36.

What is OIDC?

---

Built on top of:

```text
OAuth2
```

---

## Q37.

What does OIDC add?

---

Authentication.

---

## Q38.

Difference between:

```text
Access Token
```

and

```text
ID Token
```

?

---

## Q39.

Why do Kubernetes and cloud systems heavily use OIDC?

---

## Q40.

Can OIDC replace passwords?

---

# 🟣 Section 9 — SAML

---

## Q41.

What is SAML?

---

Enterprise federation protocol.

---

## Q42.

Difference between:

```text
OIDC
```

and

```text
SAML
```

?

---

## Q43.

Why do older enterprises still use SAML?

---

## Q44.

When would you choose OIDC over SAML?

---

## Q45.

What is Identity Federation?

---

# 🟢 Section 10 — Passkeys & FIDO2

---

## Q46.

Why are passwords considered weak?

---

Problems:

```text
Reuse
Phishing
Weak Choices
Sharing
```

---

## Q47.

What is a Passkey?

---

## Q48.

Why are passkeys resistant to phishing?

---

## Q49.

How does FIDO2 improve security?

---

## Q50.

Could passkeys eventually replace passwords?

---

# 🟡 Section 11 — Cloud Federation

---

## Q51.

Why should cloud administrators avoid long-lived credentials?

---

## Q52.

What is workload identity?

---

## Q53.

How does AWS IAM differ from Linux users?

---

## Q54.

What is identity federation in cloud environments?

---

## Q55.

Why are temporary credentials preferred?

---

# 🔴 Section 12 — Advanced Security Architecture

---

## Q56.

Design authentication for:

```text
10000 Employees
2000 Servers
Multiple Clouds
Kubernetes
VPN
```

---

## Q57.

Would you choose:

```text
LDAP
AD
FreeIPA
OIDC
SAML
```

and why?

---

## Q58.

How would you implement:

```text
Least Privilege
MFA
Auditability
High Availability
```

?

---

## Q59.

How would you secure privileged access?

---

Expected concepts:

```text
PAM
JIT Access
MFA
Audit Logs
PAM Solutions
```

---

## Q60.

What is Just-In-Time Access (JIT)?

---

# ⚫ Section 13 — Red Team Questions

---

## Q61.

How might an attacker abuse:

```text
sudo
```

misconfigurations?

---

## Q62.

How can PATH manipulation lead to privilege escalation?

---

## Q63.

Why are writable scripts dangerous when executed via sudo?

---

## Q64.

How can environment variables be abused?

---

Examples:

```text
LD_PRELOAD
PATH
LD_LIBRARY_PATH
```

---

## Q65.

Why is:

```text
NOPASSWD: ALL
```

a major risk?

---

# ⚪ Section 14 — Blue Team Questions

---

## Q66.

How would you investigate suspicious root activity?

---

## Q67.

How would you determine who restarted a service?

---

## Q68.

How would you investigate a compromised account?

---

## Q69.

How would you detect privilege escalation attempts?

---

## Q70.

What logs would you collect first?

---

# 🏆 Principal-Level Architecture Questions

---

## Q71.

Design identity for:

```text
Hybrid Cloud
Kubernetes
On-Prem Linux
Remote Workforce
```

---

## Q72.

How would you implement Zero Trust?

---

## Q73.

How would you eliminate passwords entirely?

---

## Q74.

How would you secure machine identities?

---

## Q75.

What will Linux authentication look like in 10 years?

---

# 🔥 Ultimate Expert Question

Explain the complete identity flow:

```text
User
 │
 ▼
Identity Provider
 │
 ▼
Authentication
 │
 ▼
Federation
 │
 ▼
Authorization
 │
 ▼
Linux Access
 │
 ▼
sudo
 │
 ▼
Kubernetes
 │
 ▼
Cloud IAM
 │
 ▼
Audit Logging
 │
 ▼
Zero Trust Verification
```

using:

```text
UID
GID
LDAP
Kerberos
SSSD
FreeIPA
PAM
OAuth2
OIDC
SAML
Passkeys
FIDO2
RBAC
IAM
Zero Trust
```

If you can answer this clearly, you are operating at a Staff/Principal Engineer level rather than a traditional Linux administrator.
