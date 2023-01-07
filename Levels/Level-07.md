---
author: Josh Genao
Date: 2023-01-06
Category:
Tags:
---

# Level-07
## Description
The **flag07** user was writing their very first perl program that allowed them to ping hosts to see if they were reachable from the web server.

```
#!/usr/bin/perl

use CGI qw{param};

print "Content-type: text/html\n\n";

sub ping {
  $host = $_[0];

  print("<html><head><title>Ping results</title></head><body><pre>");

  @output = `ping -c 3 $host 2>&1`;
  foreach $line (@output) { print "$line"; }

  print("</pre></body></html>");
  
}

# check if Host set. if not, display normal page, etc

ping(param("Host"));
```

## Setup
SSH into the Nebula VM using the following credentials:

```
username: level07
password: level07
```

## Summary
In the perl script we can see that the program is expecting data from the `Host` parameter:

```
ping(param("Host"));
```

We can see in the ping subroutine, the host parameter is being passed into the ping command without any input validations. This is where we can exploit the code. 

First lets determine what port the webpage is running:

![](../imgs/Pasted%20image%2020230106213553.png)
With some trial and error, we can determine that the webpage is listening on port 7007
![](../imgs/Pasted%20image%2020230106213227.png)
We can also determine the port by viewing the `thttpd.conf`
![](../imgs/Pasted%20image%2020230106213820.png)

Let's determine if we can inject our commands by doing the following: 

![](../imgs/Pasted%20image%2020230106215517.png)

## Flag

![](../imgs/Pasted%20image%2020230106215110.png)

## References
