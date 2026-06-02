# ACL Lab: Linux File Permissions & Information Flow

## Objective
To understand Linux ACLs using `getfacl` and `setfacl`, and demonstrate how permissions and execution context affect data visibility.

---

## Key Concepts

- UNIX permissions (rwx)
- ACL extensions (+)
- Default ACL inheritance
- Principle of least privilege
- Information vs file access

---

## Tools Used

- getfacl
- setfacl
- ls -l
- echo / cat
- bash scripting

---

## Lab Environment

Users:
- Alice
- Bob
- Harry

Shared directory:
- /shared_data

---

## Summary Tasks

1. Permission inspection
2. Single-file ACL modification
3. Default ACL on directory
4. Trojan-horse script exploitation
