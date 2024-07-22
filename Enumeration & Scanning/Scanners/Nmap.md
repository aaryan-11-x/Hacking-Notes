**Nmap** Stands For *Network Mapper*.

It is a free open source tool, employed to *discover hosts and services on a computer network* by sending packets and analyzing the retrieved responses.

1.  When a _privileged_ user tries to scan targets on a local network (Ethernet), Nmap uses _ARP requests_. A privileged user is `root` or a user who belongs to `sudoers` and can run `sudo`.
2.  When a _privileged_ user tries to scan targets outside the local network, Nmap uses *ICMP echo requests*, *TCP ACK* (Acknowledge) to *port 80*, *TCP SYN* (Synchronize) to *port 443*, and ICMP timestamp request.
3.  When an _unprivileged_ user tries to scan targets outside the local network, Nmap resorts to a *TCP 3-way handshake* by sending SYN packets to ports 80 and 443.

----
![[Pasted image 20240301171619.png]]
- **T4** Is The *Speed/Timing Template*. It's In A Range Of [0-5]. Higher The Number, Faster The Speed.
- **-n** to *Disable DNS Resolution*
- **-p** Is To Define The *Port Range*.  *-p-* Scans All The Ports From[0-65,535].
		*Not Defining -p* Will Scan The *Non-Ephemeral Ports*.
- **-A** Stands For *Aggressive Scan*. It Returns Everything, The OS, It's Version, Script Scanning, Traceroute, All The Possible Information it can get.
- **-Pn** To *Not Ping The Host Before Scanning* (<u>This Gives Faster Results</u>)
- **-sI** To Use *Zombie IP* (For More Info: Goto #SICommand )
- **-sn** For *Ping Sweep*
- **-sU** Also Scans The *UDP Ports*. **Don't Use -A While Doing This Else It'll Take Hours & Hours To Scan Cuz UDP Is Connectionless Protocol**.
	- If Port is Open, Nmap Will Mark it as **open|filtered**
	- If Port is Closed, an ICMP Packet With **"Port Unreachable"** is Sent by the Server. 
- **-sV** Attempts to determine the *version of the service* running on port
- **--open** Shows All The *Open Ports*
- **-O** Scans For The *Operating System* Of The Target.
- **-oN** To Save Nmap *Results in Normal Format*.
- **-oG** For *Grepable Output (Used During Pattern Matching)*
![[Pasted image 20221118203536.png]]
- **-D** Stands For Decoy, Used To *Cover Our Tracks* By Giving A Fake IP Address.
- **-F** To Scan For Only *Well Known Ports*.
- **-vv** For *Extra Verbosity*


##### Nmap Host Discovery using ICMP
- **-PE** to send *ICMP Echo Request* (Combine with **-sn**, Usually get blocked by Firewalls)
- **-PP** to use *ICMP Timestamp Request* (Combine with **-sn**)
- **-PM** to use *ICMP address mask queries* (Combine with **-sn**)


##### Nmap Host Discovery using TCP & UDP
- **-PS** to use *TCP SYN Ping* (Combine with **-sn**)
For example, `-PS21` will target port 21, while `-PS21-25` will target ports 21, 22, 23, 24, and 25. Finally `-PS80,443,8080` will target the three ports 80, 443, and 8080.
- **-PA** to use *TCP ACK Ping* (Combine with **-sn**)
For example, consider `-PA21`, `-PA21-25` and `-PA80,443,8080`. If no port is specified, port 80 is Default.
![[Pasted image 20230518202303.png]]



- **-sA** For *ACK Flag Probe Scan (Used To Analyze The Firewall & How To Bypass It)*
![[Pasted image 20221118200859.png]]

- **-sF** For *Fin Scan (Sets the FIN Flag)*
![[Pasted image 20230406094618.png]]

- **-sX** For *Xmas Scan (Sets All Flags ON)*
![[Pasted image 20221118200155.png]]

- **-sN** For *Inverse TCP Flag Scan / Null Scan*
![[Pasted image 20221118195945.png]]

- **-sS** For *Stealth Scan*
![[Pasted image 20221118195548.png]]

- **-sT** For *TCP Connect Scan*
![[Pasted image 20221118194930.png]]

**Microsoft Windows (and a lot of Cisco network devices)** are known to respond with a *RST to any malformed TCP packet -- regardless of whether the port is actually open or not. This results in all ports showing up as being closed.*

---

# Nmap Scripting Engine (NSE)
The **Nmap Scripting Engine (NSE)** is an incredibly powerful addition to Nmap, extending its functionality quite considerably. NSE Scripts are written in the *Lua* programming language, and can be used to do a variety of things: *from scanning for vulnerabilities, to automating exploits for them*. The NSE is particularly useful for reconnaisance, however, it is well worth bearing in mind how extensive the script library is.

- **--script=vuln** To *Scan For All The Vulnerabilities From The Nmap Script Docs*.
	- **--script=sniffer-detect** [Host_IP] To Detect If *Someone's Sniffing The Network*.
	- `-sC` for `--script=default`
	- To See All NMap Scripts, Go To */usr/share/nmap/scripts*

There are many categories available. Some useful categories include:
-   `safe`:- Won't affect the target
-   `intrusive`:- Not safe, Will likely to affect the target!
-   `vuln`:- Scan for vulnerabilities
-   `exploit`:- Attempt to exploit a vulnerability
-   `auth`:- Attempt to bypass authentication for running services (e.g. Log into an FTP server anonymously)
-   `brute`:- Attempt to bruteforce credentials for running services
-   `discovery`:- Attempt to query running services for further information about the network (e.g. query an SNMP server).

To run a specific script, we would use `--script=<script-name>` , e.g. `--script=http-fileupload-exploiter`.

Multiple scripts can be run simultaneously in this fashion by separating them by a *comma*. For example: `--script=smb-enum-users,smb-enum-shares`.

Some scripts require arguments (for example, credentials, if they're exploiting an authenticated vulnerability). These can be given with the `--script-args` Nmap switch. An example of this would be with the `http-put` script (used to upload files using the PUT method). This takes two arguments: *the URL to upload the file to, and the file's location on disk.* For example:
`nmap -p 80 --script http-put --script-args http-put.url='/dav/shell.php',http-put.file='./shell.php'`
Note that the *arguments are separated by commas, and connected to the corresponding script with periods* (i.e.  `<script-name>.<argument>`).


1) **ftp-anon**
	Checks if an FTP server allows *anonymous logins*. If anonymous is *allowed, gets a directory listing of the root directory and highlights writeable files*.
	

----


### More Information Below

	![[Pasted image 20221022154851.png]]
	![[Pasted image 20221022154718.png]]
	![[Pasted image 20221022155013.png]]
	![[Pasted image 20221022155419.png]]


---
