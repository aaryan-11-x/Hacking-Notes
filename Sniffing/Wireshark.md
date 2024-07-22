# T-Shark
CLI version of Wireshark

| Switch Command | Result                                                       |
| -------------- | ------------------------------------------------------------ |
| -D             | Will display any interfaces available to capture from.       |
| -i             | Selects an interface to capture from. ex. -i eth0            |
| -n             | Do not resolve hostnames.                                    |
| -x             | Show Contents of packets in hex and ASCII.                   |
| -c [no]        | Grab a specific number of packets, then quit the program.    |
| -s             | Defines how much of a packet to grab.                        |
| -r [file.pcap] | Read from a file.                                            |
| -W [file.pcap] | Write into a file.                                           |
| -P             | Will print the packet summary while writing into a file (-W) |
| -V             | Print packet details                                         |
| -Y             | Use Filters like in Wireshark                                | 


---

![[Pasted image 20231111111932.png]]
# Filters
- ip.addr == [ip_address] To Filter Out Packets With [ip_address] As *Source Or Destination*
- ip.dst == [ip_address] To Filter Out Packets With [ip_address] As *Destination Only*
- ip.src == [ip_address] To Filter Out Packets With [ip_address] As *Source Only*
- `dns.qry.name` to filter by DNS queries
- `tcp.port / udp.port [no]` to filter out *ports*
- `_ws.col.protocol == "HTTP"` to filter packets based on *protocol*
- `frame contains` "[string]" to filter by *info*
- pkt_comment To Show *Comments*
- http.request.method == [POST/GET]
- ip.geoip.country == [country name] to Filter Packets by Country
```wireshark
ip.geoip.country == "United States"
```

- Filter by *packet length*
```wireshark
frame.len == 243    <-- Use this first
ip.len == 229
udp.length == 209
data.len == 201
```

- Filter by *traffic type*
```wireshark
broadcast
multicast
unicast
```

%%Use Logic Gates (C Programming वाले) For Applying Multiple Filters%%

![[Pasted image 20230514130300.png]]

----
### TLS Handshake via HTTPS
![[Pasted image 20231111142543.png]]
In the first few packets, we can see that the client establishes a session to the server using port 443 <mark style="background: #ADCCFFA6;">boxed in blue</mark>. This signals the server that it wishes to use HTTPS as the application communication protocol.

Once a session is initiated via TCP, a TLS `ClientHello` is sent next to begin the TLS handshake. During the handshake, several parameters are agreed upon, including session identifier, peer x509 certificate, compression algorithm to be used, the cipher spec encryption algorithm, if the session is resumable, and a 48-byte master secret shared between the client and server to validate the session.

Once the session is established, all data and methods will be sent through the TLS connection and appear as TLS Application Data <mark style="background: #FF5582A6;">as seen in the red box</mark>. TLS is still using TCP as its transport protocol, so we will still see acknowledgment packets from the stream coming over port 443.

---
# Capture Sensitive Data

![[Pasted image 20230215205233.png]]
![[Pasted image 20230215205159.png]]


When You Follow The *Stream*, ==Red== Colour Is The Data From ==Client Side== & <mark style="background: #ADCCFFA6;">Blue</mark> Colour Is The Data From <mark style="background: #ADCCFFA6;">Server Side</mark>
![[Pasted image 20230215222139.png]]

---
# Statistics & Analysis
![[Pasted image 20231114091229.png]]

---
# Extract Files
- Works For Protocols : **DICOM, FTP, TFTP, HTTP, IMF, SMB**
- Go To **File -> Export Objects**
- Select The Desired Protocol!

##### Exporting Files From Packets

![[Pasted image 20230215222545.png]]


---
# Shortcuts
- To Follow **TCP Stream** : CTRL + SHIFT + ALT + T
- To Follow **UDP Stream** : CTRL + SHIFT + ALT + U
- To Capture the **PCAP File Properties** : CTRL + ALT + SHIFT + C    (From Anywhere)
- To **Find Packets** : CTRL + F

---
# Decrypting RDP Connections
You need an **RSA Private Key** for decrypting RDP connection!
1) go to Edit → Preferences → Protocols → TLS
2) On the TLS page, select Edit by RSA keys list → a new window will open.
![[Pasted image 20231114100803.png]]

3) Click the + to add a new key
4) Type in the IP address of the RDP server `IP`
5) Type in the port used `3389`
6) Protocol filed equals `tpkt` or `blank`
7) Browse to the `server.key` file and add it in the key file section.
8) Save and refresh your pcap file.


## Get Private key from .pki file
https://www.hackingarticles.in/wireshark-for-pentester-decrypting-rdp-traffic/


## Replay RDP Session
1. Export the PDUs from Wireshark by selecting `File > Export PDUs and selecting OSI Layer 7` and save that to a file
2. Convert it to a replay file using the provided `pyrdp-convert` utility: 
```sh
pyrdp-convert.py --src 192.168.110.1 -o MonsterIncTheMiddle2Replay MonsterIncTheMiddle2PDUs.pcapng

# Decrypt SSL Packets if encrypted with ssl.log file
pyrdp-convert.py –src 10.2.0.198 –secrets ssl.log -o path/to/output capture.pcap
```
3. Open the replay file with `pyrdp-player`:
```sh
pyrdp-player.py MonsterIncTheMiddle2Replay
```
4. Watch the replay

![[Pasted image 20231204230255.png]]



---
# CTF
1) `pkt.comment`
2) `strings` on the pcapng file
3) `binwalk -e capture.pcap --run-as=root`
4) `foremost thuder.pcap` to extract images 
5) Follow TCP stream (tcp.stream eq [number]) && **Shortcut**
6) `ftp.request.command` to show commands run across ftp
7) `ftp-data` to show the data transferred over ftp **port 20**
8) Extract all data from Packets
```python
#!/usr/bin/env python
from scapy.all import *

packets = rdpcap("filename.pcap")
for p in packets:
	print(p.show())
```


https://github.com/welchbj/ctf/blob/master/docs/pcap.md

