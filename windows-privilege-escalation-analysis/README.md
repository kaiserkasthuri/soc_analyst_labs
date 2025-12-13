# Windows Previlege Escalation Analysis
> This investigation was completed as part of a hands-on lab from Blue Team Labs Online (BTLO).

## Senario 

<img width="1742" height="369" alt="image" src="https://github.com/user-attachments/assets/bf65d698-c8d5-4cf1-a46f-23b1c29fdaf7" />

## Overview

This lab looks at the attacker's bash history to understand what happened on the server. There were no signs of anyone logging in, so the attacker didn’t gain access through SSH. Instead, they took advantage of a hidden weakness in the website. They uploaded a malicious file called x.phtml, which gave them remote access and allowed them to look for vulnerabilities on the server. Through this file, they discovered that Python had the SUID bit enabled, which they used to escalate their privileges and eventually gain root access. Afterward, the attacker deleted x.phtml to hide their tracks and make the attack harder to detect.


## Tools Used
NotePad ++

## What user (other than ‘root’) is present on the server?

I discovered that the attacker accessed Daniel's home directory.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6b597852-93bd-48c0-a8cf-6da61c14b050" />

## What script did the attacker try to download to the server?

The attacker downloaded ```linux-exploit-suggester.sh,``` which is a well-known script attackers use to:

- Scan the server
- Identify kernel vulnerabilities
- Help them escalate privileges to root

Attacker downloaded the file as ```les.sh```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cc1efe7d-6bba-49f9-a7fb-65b3cd88c0ed" />

## What packet analyzer tool did the attacker try to use? 

The attacker used tcpdump to capture the server’s network traffic so they could steal data, identify new targets, and avoid detection.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b5386466-cdfe-4ddf-9ed2-81e8d6a138a1" />

## What file extension did the attacker use to bypass the file upload filter implemented by the developer?

Attacker used ```x.phtml``` malicious file to remote access through the website.

They used this website to:
- Run commands
- explore directories
- To gain high previlage
- Eventually become the root

After they were done with their work, they deleted it because:
- Makes the attack harder to detect
- Hides evidence
- Make it looks nothing happened

```rm /var/www/html/uploads/x.phtml```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5f29a38b-86e1-42c7-ba05-6a2252e2ad09" />

## Based on the commands run by the attacker before removing the php shell, what misconfiguration was exploited in the ‘python’ binary to gain root-level access? 1- Reverse Shell ; 2- File Upload ; 3- File Write ; 4- SUID ; 5- Library load 

```/usr/bin/python -c 'import os; os.execl("/bin/sh", "sh", "-p")'``` In this case, having SUID enabled is a major vulnerability for the server. Since Python runs with root privileges, the attacker can also gain root access by executing Python.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/61aab1f9-6c2c-4ada-85a4-924209a01d81" />

## Conclusion

Working through this lab helped me see how an attacker can slowly take control of a system without logging in normally. By looking at the bash history, I was able to follow what the attacker did step by step and understand how they moved from uploading a file to gaining root access. This lab made it clear how dangerous small misconfigurations, like an SUID-enabled Python binary, can be. It also helped me get better at thinking through an attack from a defender’s point of view and understanding what to look for during an investigation.









