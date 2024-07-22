# ifconfig
inet : IPv4 Address (**Private**)
inet6 : IPv6 Address
*ether : MAC Address*
	- The First 3 Pairs Of *ether* Identify The Company (Do A Lookup On Google For The MAC Address To Find The Company) 
netmask : Subnet Mask
*broadcast : Broadcast IP* (**Last IP Address Of The Network**)

***lo : Loopback Address***

![[Pasted image 20220617165936.png]]

---
# To Disable Network Interface
	ifconfig [interface] down
	OR
	ip link set eth0 up


# To Activate Network Interface
	ifconfig [interface] up


# To add a Default Gateway
```shell-session
sudo route add default gw [default gateway] eth0
```


# To Change Your IP Address
	ifconfig [interface] [IP_Address]
	Ex: ifconfig eth0 10.10.45.32
	
	 - This Will Also Change Your Subnet Mask According To Your IP
	 - First Disconnect From The Interface, Then Change & Connect Again To Your Interface


# Assign a Netmask to an Interface
```shell
ifconfig eth0 netmask 255.255.255.0
```

# Editing DNS Settings
```shell
vi /etc/resolv.conf

nameserver 8.8.8.8
nameserver 8.8.4.4
```


# To Save these Settings Permanently
```shell
vim /etc/network/interfaces
```

```text
auto eth0
iface eth0 inet static
  address 192.168.1.2
  netmask 255.255.255.0
  gateway 192.168.1.1
  dns-nameservers 8.8.8.8 8.8.4.4
```

Then, Restart Networking Service :-
```shell
systemctl restart networking
```


---

**For The Subnetting Cheatsheet, Visit :-**![[Subnet-Guide 1.xlsx]]