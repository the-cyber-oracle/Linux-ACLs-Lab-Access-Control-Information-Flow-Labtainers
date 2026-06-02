# Linux Access Control Lists (ACLs): Permission Management and Information Flow Analysis

## Overview

This repository documents a hands-on security laboratory focused on Linux Access Control Lists (ACLs), discretionary access control, and information flow security. The project explores how ACLs extend traditional UNIX permission models and demonstrates how misused trust relationships can lead to unintended disclosure of sensitive information.

Through a series of practical exercises, the lab investigates file permissions, ACL inheritance, least privilege enforcement, and a controlled Trojan Horse scenario illustrating the distinction between direct file access and indirect information access.

---

## Research Objectives

The primary objectives of this project are to:

* Analyze Linux file permissions and ACL behavior.
* Examine how ACLs extend traditional UNIX access control mechanisms.
* Implement user-specific access permissions on files.
* Configure and validate default ACL inheritance on directories.
* Investigate information leakage through trusted execution contexts.
* Demonstrate how security controls can be bypassed through user behavior rather than technical vulnerabilities.

---

## Environment

### Platform

* Linux (Labtainers Environment)
* Bash Shell

### Tools

* `ls`
* `cat`
* `echo`
* `getfacl`
* `setfacl`
* Bash scripting

### Users

| User  | Purpose                                         |
| ----- | ----------------------------------------------- |
| Alice | Authorized user with limited access             |
| Bob   | User attempting to obtain protected information |
| Harry | Additional user for permission validation       |

---

# Project Structure

```text
acl-lab/
│
├── README.md
│
├── docs/
│   ├── 01-permissions-analysis.md
│   ├── 02-single-file-acl.md
│   ├── 03-directory-default-acl.md
│   ├── 04-trojan-horse-analysis.md
│   └── concepts.md
│
├── scripts/
│   ├── bob_fun_original.sh
│   └── bob_fun_modified.sh
│
├── evidence/
│   ├── ls_outputs.txt
│   └── getfacl_outputs.txt
│
└── notes/
    └── observations.md
```

---

# Technical Analysis

## Phase 1: ACL Discovery and Permission Enumeration

The initial phase focused on identifying differences between standard UNIX permissions and ACL-enhanced permissions.

During analysis, the following permission structure was observed:

```text
-rw-rw----+
```

The trailing `+` indicated the presence of Access Control Lists (ACLs).

ACL entries were examined using:

```bash
getfacl accounting.txt
```

This revealed user-specific permissions not visible through standard `ls -l` output.

### Key Finding

Traditional permission analysis alone may not accurately represent effective access rights. ACL inspection is required for complete access-control auditing.

---

## Phase 2: User-Specific File Access

ACLs were used to grant selective read access to a protected file without modifying ownership or group membership.

Example:

```bash
setfacl -m u:alice:r file.txt
```

### Key Finding

ACLs provide granular access control while preserving the existing permission model.

---

## Phase 3: Directory ACL Inheritance

Default ACLs were configured on a directory to ensure newly created files automatically inherited access permissions.

Example:

```bash
setfacl -d -m u:bob:r /shared_data/alice
```

### Validation

New files created within the directory inherited the ACL configuration and became readable by Bob without additional administrative action.

### Key Finding

Default ACLs significantly simplify permission management in collaborative environments while reducing administrative overhead.

---

## Phase 4: Trojan Horse Demonstration

The final exercise examined a classic Trojan Horse scenario.

### Scenario

Bob lacked direct permission to access a protected file:

```text
/shared_data/accounting.txt
```

To obtain the information, Bob modified an ASCII-art script likely to be executed by Alice.

When Alice executed the script:

1. The script ran with Alice's permissions.
2. The protected data became accessible to the running process.
3. Information was copied into a location inheriting ACL permissions that allowed Bob to read it.

### Critical Observation

Bob never obtained permission to access the original file.

Instead, Bob gained access to the information contained within the file.

This distinction highlights a fundamental security principle:

> Access controls protect objects, but user actions can unintentionally expose the information those objects contain.

---

# Security Concepts Demonstrated

## Access Control Lists (ACLs)

ACLs extend traditional UNIX permissions by enabling user-specific access rights.

## Principle of Least Privilege

Users should possess only the permissions necessary to perform authorized tasks.

## Permission Inheritance

Default ACLs allow organizations to enforce consistent access policies across newly created resources.

## Information Flow Security

Protecting data requires controlling both access to files and movement of information between trust boundaries.

## Trojan Horse Attacks

Trusted users can unknowingly execute software that performs actions using their own privileges.

---

# Lessons Learned

* ACLs provide significantly more flexibility than traditional UNIX permissions.
* Effective permission auditing requires both `ls` and `getfacl`.
* Default ACL inheritance can automate secure collaboration workflows.
* Users frequently represent the weakest link in access-control systems.
* Information disclosure can occur even when direct file permissions remain secure.

---

# Future Research

Potential extensions of this project include:

* POSIX ACL administration at scale
* SELinux policy integration
* AppArmor access control analysis
* Mandatory Access Control (MAC) models
* Insider threat simulations
* Privilege escalation detection
* Secure software execution environments

---

# Author

Cybersecurity Research Project

Topics:
Linux Security • Access Control • ACLs • Information Security • Information Flow Analysis • Privilege Management • Security Operations • Cyber Defense
