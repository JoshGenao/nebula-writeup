---
author: Josh Genao
Date: 2023-01-03
Category:
Tags:
---

# Level-06
## Description
The **flag06** account credentials came from a legacy unix system.
## Setup
SSH into the Nebula VM using the following credentials:

```
username: level06
password: level06
```

## Summary
The description contains a hint that the account came from a legacy unix system. Historically, before `/etc/shadow` was used `/etc/passwd` contained the hash of the user password. We can check to see if the user account in `/etc/passwd` contains the password hash.

![](../imgs/Pasted%20image%2020230104234432.png)
Sure enough it does. We can crack this password hash using John the Ripper. Lets store this into a file and on the Kali machine lets use the secure copy command to get it out the nebula VM.

```
level06@nebula:~$ cat /etc/passwd | grep flag06 > output.txt
```

![](../imgs/Pasted%20image%2020230104235516.png)

Run John the Ripper on the `output.txt` and we will see the password is **hello**. Use this password to login as flag06.

![](../imgs/Pasted%20image%2020230104235713.png)

## Flag
![](../imgs/Pasted%20image%2020230104235151.png)
## References
