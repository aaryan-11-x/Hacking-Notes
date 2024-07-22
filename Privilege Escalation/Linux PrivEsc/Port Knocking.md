Default Location: `/etc/knockd.conf`

![[Pasted image 20231117191022.png]]
# Exploitation
https://sushant747.gitbooks.io/total-oscp-guide/content/port_knocking.html

Nmap Script Fix:-
```sh
for x in 4000 5000 6000; do nmap -Pn --host-timeout 201 --max-retries 0 -p $x [server_ip_address]; done
```