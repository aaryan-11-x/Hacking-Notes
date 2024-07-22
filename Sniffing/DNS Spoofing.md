- **Firstly**, Do **ARP Spoofing**, To Know How Do It, Check [[DHCP Starvation & Rogue DHCP Server Attack]]
- **Create a Host File** Which Will *Redirect The Desired Web Page* To Your ***Home Page***
![[Pasted image 20230212125533.png]]
	Here, **Asterik** Is a ***Wildcard!***

- Start **apache2** Server :-
	service apache2 start

- In **/var/www/html**, There Is **index.html** Which Is The *Website Attacker Will See After Successfully Spoofing It!*

- Then, Use **dnsspoof**

##### dnsspoof
Syntax :-
**dnsspoof** **-i** [network_interface] **-f** [hostfile]
![[Pasted image 20230212125818.png]]


***NOTE : This Works Only On Insecure http sites. Not On https websites!***