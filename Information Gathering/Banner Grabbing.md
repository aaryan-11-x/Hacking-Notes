Banner Grabbing Or OS Fingerprinting Is A Method To *Determine* The ***OS*** or ***Software Version*** Running On a *Remote* Target System.

There Are 2 Types Of Banner Grabbing : **Active & Passive**.
![[Pasted image 20221120113413.png]]![[Pasted image 20221120113437.png]]
It Allows The Hacker To Determine The *Vulnerabilites* The System Possesses & Exploit Them Respectively.

###### Methods For Banner Grabbing :-
1) Wappalyzer
2) netcraft![[Pasted image 20221120115634.png]]
3) [[Netcat & Ncat]]
	 nc **-nv** [IP Address]  [Port Number]
	 - If Port *80* Is *Open* :-
	 Generate An <u>HTTP Error</u> To Get The Banner![[Pasted image 20221120133802.png]]
									 OR
	echo "" | nc **-nv** [IP Address] 80
	(This Method Works For All Ports)
	
	 - If Port *22* Is *Open* :-![[Pasted image 20221120134106.png]]
	 - If Port *21* Is *Open* :-![[Pasted image 20221120134356.png]]