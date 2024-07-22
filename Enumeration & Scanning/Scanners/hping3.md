#### It's A Tool Like [[Nmap]]

hping3 **--scan** 0-1023 [ip]
To <u>Input Multiple Port Ranges</u> :-
hping **-8** 0-1023,2000-3000 [ip]
![[Pasted image 20221119182815.png]]

- **--icmp or -1** : Enable *ICMP Mode*
- **--scan or -8** : To Scan The *Target Machine*.
- **-d** : Packet *Size*.
- **-S** : Enable *SYN Flag*.![[Pasted image 20221119181403.png]]

- **-A** : Enable *ACK Flag* (Don't Use This)
- **-F** : Enable *FIN Flag*
	*Not Responding Ports* Mean That The Ports Are *Open*
![[Pasted image 20221119181623.png]]
- **-R** : Enable *RST Flag*
- **-U** : Enable *URG Flag*
- **-FUP** : Enable *FIN, URG & PSH Flags (Xmas Scan)*
- **-V** : Enable *Verbosity*
- **-c** : Packet *Count*
- **-p** : Port Number To *Connect With/DOS On*
- **--flood** : Send *Packets As Fast As Possible* ***(Used In DOS Attacks)***
![[Pasted image 20230224194530.png]]
In [[Sniffing/Wireshark]], *ICMP Packets Will Be Flooding*!

