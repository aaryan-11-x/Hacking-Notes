# DHCP Starvation Attack

![[Pasted image 20230207223018.png]]
![[Pasted image 20230207223046.png]]


# Rogue DHCP Server Attack

![[Pasted image 20230207223208.png]]
![[Pasted image 20230207223258.png]]


### Attack In Practical

**arpspoof** **-i** [network interface] **-t** [Target_IP]  [Default Gateway]
- First, Do It For *Default Gateway*
![[Pasted image 20230208191643.png]]

- Then, Do It The Same In *Reverse*
**arpspoof** **-i** [network interface] **-t** [Default Gateway]  [Target_IP]
![[Pasted image 20230208192040.png]]

- This Indicates *MAC* Has Been *Spoofed*
	On Linux
![[Pasted image 20230208192649.png]]
	On Windows
![[Pasted image 20230208192210.png]]

- ***Very Important!!!!***, **Do Port Forwarding**
![[Pasted image 20230208194133.png]]
- Use **tcpdump port** [port_number] To Monitor All The *User Activites On That Specific Port*
- You Can Do This By Using [[Ettercap]] Too!