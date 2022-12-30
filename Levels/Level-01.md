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

## Flag

## References
