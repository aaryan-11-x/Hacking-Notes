Shijack is a TCP connection hijacking tool for Linux, FreeBSD, and Solaris. Works For Traffic Going Through *Unencrypted Sessions Like Telnet*

1) Sniff The Target System, For Practical Go to [[DHCP Starvation & Rogue DHCP Server Attack]]
2) The Target System Should Have a TCP Session Open (*Telnet* For Example)
3) Open [[Wireshark]] & Start Capturing Packets!
	- Filter For *Telnet Packets* & Packets Going From <mark style="background: #FF5582A6;">Target IP To Server IP</mark>
	- ip.src == [target_ip] && ip.dst == [server_ip]
4) Note Down The **Source Port** Of The Last *Telnet Packet* ![[Pasted image 20230325120756.png]]

5) **Syntax For Shijack** :-
	./shijack-lnx [interface]  [target ip]  [source port]  [telnet_server ip]  [dst port]


6) Enter Something & ***BOOM!*
On Our Attack Machine!
![[Pasted image 20230325121705.png]]


**On <mark style="background: #FF5582A6;">Target System</mark>**
![[Pasted image 20230325121743.png]]

**On <mark style="background: #ADCCFFA6;">Server Side</mark>**
![[Pasted image 20230325121856.png]]