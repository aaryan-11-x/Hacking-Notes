1) *arp-scan -l*
	Lists The Connected Devices To Your Network
![[Pasted image 20220712165944.png]]


2) *netdiscover -r* [iprange/CIDR]
	Also Does The Same Thing As The Above Command
![[Pasted image 20220712170607.png]]


3) *for /L %i in (1,1,254) do ping 192.168.1.%i -n 1 -w 100*
	Use This In Windows To Detect Live Hosts In a Network
	![[Pasted image 20221022165335.png]]