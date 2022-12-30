---
author: Josh Genao
Date: 2022-12-29
Category:
Tags:#setuid
---

# Level-00
## Description
This level requires you to _find_ a Set User ID program that will run as the “flag00” account. You could also find this by carefully looking in top level directories in / for suspicious looking directories.
## Setup
- SSH into the Nebula VM using the following credentials:
```
username: level00
password: level00
```
## Summary
## What is setuid and setgid?
The Unix access rights flags **setuid** and **setgid** (short for _set user identity_ and _set group identity_)[^1] allow users to run an executable with the file system permissions of the executable's owner or group respectively and to change behaviour in directories. They are often used to allow users on a computer system to run programs with temporarily elevated privileges in order to perform a specific task. While the assumed user id or group id privileges provided are not always elevated, at a minimum they are specific.

The flags `setuid` and `setgid` are needed for tasks that require different privileges than what the user is normally granted, such as the ability to alter system files or databases to change their login password.
## How to Find Files with setuid Permissions?
`$ find / -user flag00 -perm -4000 2>/dev/null`
## Flag
![](../imgs/Pasted%20image%2020221229235301.png)
## References
[^1]: https://en.wikipedia.org/wiki/Setuid
