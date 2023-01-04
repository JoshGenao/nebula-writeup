---
author: Josh Genao
Date: 2023-01-02
Category:
Tags:
---

# Level-05
## Description
Check the **flag05** home directory. You are looking for weak directory permissions
## Setup
SSH into the Nebula VM using the following credentials:

```
username: level05
password: level05
```
## Summary

If we look at the `/home/flag05` directory, we will notice that the `.backup` contains the +755 directory permission. Inside that directory we will find a TAR archive file in which we are able to read the conents. Since we cannot extract the contents on that folder we can copy the `backup-19072011.tgz` file into our `/home/level05` directory

![](../imgs/Pasted%20image%2020230103224045.png)
Extract the Tar archive and we will get a copy of the .ssh and priv and pub keys!

![](../imgs/Pasted%20image%2020230103224454.png)

We can ssh as the `flag05` user doing the following command.

```
level05@nebula:~/tmp/.ssh$ ssh -i id_rsa flag05@localhost
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is ea:8d:09:1d:f1:69:e6:1e:55:c7:ec:e9:76:a1:37:f0.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.

      _   __     __          __
     / | / /__  / /_  __  __/ /___ _
    /  |/ / _ \/ __ \/ / / / / __ `/
   / /|  /  __/ /_/ / /_/ / / /_/ /
  /_/ |_/\___/_.___/\__,_/_/\__,_/

    exploit-exercises.com/nebula


For level descriptions, please see the above URL.

To log in, use the username of "levelXX" and password "levelXX", where
XX is the level number.

Currently there are 20 levels (00 - 19).


Welcome to Ubuntu 11.10 (GNU/Linux 3.0.0-12-generic i686)

 * Documentation:  https://help.ubuntu.com/
New release '12.04 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.
```

## Flag
![](../imgs/Pasted%20image%2020230102232142.png)
## References
