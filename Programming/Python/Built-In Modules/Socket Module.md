Socket programming is a way of *connecting two nodes on a network to communicate with each other*. One socket(node) listens on a particular port at an IP, while the other socket reaches out to the other to form a connection.

![[Pasted image 20220621220749.png]]

1) **socket.socket()**
	Creates A Socket

2) socket.bind ([host, port])
	Binds The Host & Port To The Socket. In Client Side, Use *accept instead of bind*

3) socket.send()
	Sends Message To The Other Computer

4) socket.listen([Number Of Bad Connections Before Throwing An Error])
	Listens For The Signal

5) socket.recv()
	Decodes The Message Into It's System

6) socket.accept()
	###### Accepts The Connection & Returns 2 Variables, A Connection & A List

7) socket.gethostbyname([hostname])
	###### Takes The DNS Name & Converts It Into IPv4 Address



Below Is A Diagram For Making A Reverse Shell Using Socket Module
![[Pasted image 20220626195308.png]]

