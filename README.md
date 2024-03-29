# Privilege Escalation

Privilege escalation is the elevation of permissions to gain leveraged access to sensitive resources on a computer system or network. Typically, credentials are provided to access privileged users; however, attackers with compromised access can exploit bugs, design flaws, and configuration oversights in system infrastructure to obtain and masquerade as higher privileged accounts. With these accounts, attackers can maintain an undetected presence in the system by masking IP addresses and removing logs; or damaging the organisation by exfiltrating or corrupting data.

Vertical privilege escalation, or privilege elevation, enables attackers to elevate permissions to access higher privileged users. For instance, exploiting vulnerabilities to move from an ordinary employee account to a network administrator.

Horizontal privilege escalation, or account takeover, allows attackers to move laterally to similarly privileged accounts. For instance, moving from the accounts of one network administrator to another. 

## Linux Privilege Escalation

The following links provide background on each privilege escalation method and a walkthrough for each challenge on TryHackMe's Linux Privilege Escalation room.

- [Kernel exploits](https://github.com/KayEm06/Linux-privilege-escalation/blob/main/Methods/Kernel%20exploits.md)
- [Super User Do (Sudo)](https://github.com/KayEm06/Linux-privilege-escalation/blob/main/Methods/Sudo.md)
- [Set User ID (SUID)](https://github.com/KayEm06/Linux-privilege-escalation/blob/main/Methods/SUID.md)
- [Capabilities](https://github.com/KayEm06/Linux-privilege-escalation/blob/main/Methods/Capabilities.md)
- [Cron jobs](https://github.com/KayEm06/Linux-privilege-escalation/blob/main/Methods/Cronjobs.md)
- [PATH](https://github.com/KayEm06/Linux-privilege-escalation/blob/main/Methods/PATH.md)
- [Network File System (NFS)](https://github.com/KayEm06/Linux-privilege-escalation/blob/main/Methods/Network%20File%20System.md)
