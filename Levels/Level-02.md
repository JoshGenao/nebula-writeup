---
author: Josh Genao
Date: 2022-12-31
Category:
Tags:
---

# Level-02
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
  char *buffer;

  gid_t gid;
  uid_t uid;

  gid = getegid();
  uid = geteuid();

  setresgid(gid, gid, gid);
  setresuid(uid, uid, uid);

  buffer = NULL;

  asprintf(&buffer, "/bin/echo %s is cool", getenv("USER"));
  printf("about to call system(\"%s\")\n", buffer);
  
  system(buffer);
}
```
## Setup
-   SSH into the Nebula VM using the following credentials:

```
username: level02
password: level02
```
## Summary
The program reads in the `USER` environment variable and stores it into a buffer. The buffer  includes  the following information:
- `/bin/echo ` + `USER`  + ` is cool`
We can change the `USER` environment variable to whatever we like as shown below:

![](../imgs/Pasted%20image%2020221231143327.png)
In order to exploit the code above, we just need to change the `USER` environment variable   which includes the `getflag` . This can be done by using a semi-colon to separate the commands. Piping can also be used.
## Flag

![](../imgs/Pasted%20image%2020221231144807.png)

## References
