---
author: Josh Genao
Date: 2023-01-06
Category:
Tags:
---

# Level-08
## Description
World readable files strike again. Check what that user was up to, and use it to log into **flag08** account.
## Setup
SSH into the Nebula VM using the following credentials:

```
username: level08
password: level08
```
## Summary
Copy over the pcap file to a machine that contains Wireshark.
Run the following command to analyze the pcap file
```
sudo wireshark capture.pcap
```

To follow the protocol stream do the following:
	Right click > Follow > TCP Stream

Change "Show Data as:" **Hex Dump**

We will be given the following data:

![](../imgs/Pasted%20image%2020230106224604.png)

The user seemed to enter `backdoor...00Rm8.ate.` but if we look closely on the hexadecimal values, we will notice that the `.` are actually `7f` which represents the delete character in ASCII. So the password will actually be:
`backd00Rmate`
## Flag
![](../imgs/Pasted%20image%2020230106224751.png)
## References
