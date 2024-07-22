pspy is a command line tool designed to *snoop on processes without need for root permissions*. It allows you to see commands run by other users, cron jobs, etc. as they execute. *Great for enumeration of Linux systems in CTFs*. Also great to demonstrate your colleagues why passing secrets as arguments on the command line is a bad idea.

1) First, Download The pspy File Of The *Desired System Architecture*.
2) Create A *LocalHost Server And Download The pspy File From There Using wget or curl*.
	**wget** http://192.168.2.25:8000/pspy64
![[Pasted image 20220809104710.png]]
4) There, You Can Find *Background Processes*.![[Pasted image 20220809104834.png]]

```sh
# To exit out of shell after 2 minutes
timeout 120s ./pspy
```