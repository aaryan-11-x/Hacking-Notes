*DNS reconnaissance* is part of the information gathering stage on a penetration test engagement.*When a penetration tester is performing a DNS reconnaissance, he is trying to obtain as much as information as he can regarding the DNS servers and their records*.The information that can be gathered it can disclose the network infrastructure of the company without alerting the IDS/IPS.

###### To find subdomains
https://crt.sh/
```sh
# Search Syntax
%.deepcytes.io
```

The types of enumeration that performs include the following:
-   Zone Transfer
-   Reverse Lookup
-   Domain and Host Brute-Force
-   Standard Record Enumeration (wildcard,SOA, MX[Mail Exchanger], A, TXT etc.)
-   Cache Snooping
-   Zone Walking
-   Google Lookup

```sh
# Basic commands
dnsrecon -d hackersploit.org

# Bruteforce subdomains
dnsrecon -d megacorpone.com -D ~/list.txt -t brt
```

1) If You See A *DNS Port Open (Like 53), Use The Following Command :-*
![[Pasted image 20220813105647.png]]

**-r : IP range for reverse lookup brute force in formats <u>(first-last) or in (range/bitmask)</u>
-n : IP Address Of The Server
-d : Domain**

*blackpearl.tcm* Is The <u>Pointer Record (PTR)</u>

Then, *Change The DNS Configuration* By Going Into The `/etc/hosts` Folder
![[Pasted image 20220813110227.png]]


2) Using ***host*** Command
- By default, looks for `A, AAAA, MX` records
	- To Identify The *Name Server* :-
			host **-t** ns [Domain Name]
			![[Pasted image 20221203124424.png]]
	- To Get The ***Zone File*** :-
			host **-l** [Domain Name]  [Name Server]![[Pasted image 20221203124842.png]]
		Syntax :-
		 - **-t** : Specifies The *Query Type (ns, ptr, mx And MUCH MORE)*
		 - **-l** : Lists *All Hosts* in a Domain Using *AXFR*


3) Using ***nslookup*** Command
```sh
# Write domain name and then IP of the domain to prevent timeout errors
nslookup -type=TXT megacorpone.com 192.168.176.151
```
	
	![[Pasted image 20221203125607.png]]
	For Advanced Searching With ***nslookup***
		First, nslookup then hit **'Enter'**
		Then, For ns : **set type=ns**
		Then, [Domain Name]
		Also Try For **type=cname**
 ![[Pasted image 20221203130617.png]]

Use ***nslookup*** On *Windows*, It Gives **More Information**
![[Pasted image 20221203140333.png]]


4) Using ***dig (Domain Information Groper)*** **(Most Important)**

- **-4** : Use *IPv4 Query*
- **-6** : Use *IPv6 Query*
- **-p** : Specify *Port Number*
- **-q** : Specify *Query Name*
- **-t** : Specify *Query Type*
	- **ns** : Name Server
	- **mx** : Mail Exchange
- **+short** : For *Short Output*
	![[Pasted image 20221203141245.png]]![[Pasted image 20221203141432.png]]

	To Get The *Zone File (Zone Transfer)* :-
	dig AXFR @[Name Server]  [Domain Name]
	Similar Output To nslookup On Windows!
	![[Pasted image 20221203141938.png]]


5) **whois**
```sh
whois zonetransfer.me

# Connect with a server host
whois megacorpone.com -h 192.168.176.251
```

![[Pasted image 20240210222153.png]]


6) **fierce**
DNS scanner that helps locate non-contiguous IP space and hostnames against specified domains.
```sh
fierce --domain [domain]
```


7) **knockpy**
Used for subdomain brute forcing, better than fierce
```sh
knockpy -d zonetransfer.me

# -w : Specify wordlist (Else uses it's own wordlist)
# -t : Timeout
```


8) **dnsenum**
Brute-force sub-domains & get other info about them like IP address, DNS records
```sh
dnsenum megacorpone.com
```