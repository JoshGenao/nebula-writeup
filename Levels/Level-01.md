---
author: Josh Genao
Date: 2022-12-29
Category:
Tags:
---

# Level-01
## Description
There is a vulnerability in the below program that allows arbitrary programs to be executed, can you find it?

```
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>
#include <stdio.h>

int main(int argc, char **argv, char **envp)
{
  gid_t gid;
  uid_t uid;
  gid = getegid();
  uid = geteuid();

  setresgid(gid, gid, gid);
  setresuid(uid, uid, uid);

  system("/usr/bin/env echo and now what?");
}
```
## Setup
- SSH into the Nebula VM using the following credentials:
```
username: level01
password: level01
```
## Summary
The following example program violates the CERT C Secure Coding Standard rule [ENV33-C](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=87152177). In this case, an attacker can modify the `PATH` variable and use the `system()` function  to run arbitrary code.

An attacker can manipulate the `/usr/bin/env` such that this program can execute an `echo` program that was created by an attacker. The attacker would add the path to the newly created echo program into to the Linux `PATH` variable.

This can be done by doing the following:
1. We first verify the environment variables by calling the `/usr/bin/env` command
2. Notice that the `PATH` varable is printed out. This variable contains an ordered list of paths that Linux will search for executables when running a command. Therefore, we can add to the beginning of `PATH` list so it can run our echo script and not the default Linux echo program.

![](../imgs/Pasted%20image%2020221231124848.png)
3. First lets create a bash script named `echo` in the `/tmp/` folder. This script will contain a call to the `getflag` command. 
4. Make the `/tmp/echo` file executable by using `chmod +x /tmp/echo`

![](../imgs/Pasted%20image%2020221231125638.png)
5. Append a new path at the beginning of the `PATH` variable. The reason we want to append to the beginning of the `PATH` variable is so our `echo` command will appear first in the list as shown below

![](../imgs/Pasted%20image%2020221231130501.png)
![](../imgs/Pasted%20image%2020221231130546.png)
6. Run the flag01 program
## Flag
![](../imgs/Pasted%20image%2020221231131509.png)
## References
