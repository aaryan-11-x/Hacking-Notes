# Bypass Firewalls Using [[Nmap]]

1)  **By Changing The MTU (Firewall Allows Smaller Packets)**
	nmap --mtu [16 (Bytes)]  [IP]
					OR
	nmap -f 16 [IP]

	###### To Change The Packet Length
	nmap --data-length [length, Say 40]  [Target_IP]


2) **By Changing Your IP (If It's Blocked) & MAC Address**
	nmap -D RND:16 [IP]
				OR
	nmap -Pn -e [network_type] -sI [Zombie_IP]  [Target_IP]             #SICommand
	(For This, Make Sure That Firewall Of The Zombie Machine Isn't Blocking The Requests)
	###### To Change The Source Port :-
		nmap --source-port [Port Number] [IP]
	
![[Pasted image 20221118214058.png]]


# Spoof MAC Address

```sh
nmap -sT -Pn --spoof-mac [0 For Randomly Generated MAC Address]  [Target IP]
```

<u>-sT & -Pn</u> Are Important Else You Won't Get The Results, Since *MAC Address Has Been Changed, It Sends The Result To That MAC Address.*