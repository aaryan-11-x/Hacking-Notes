### snmp-check 
snmp-check [Target IP]
![[Pasted image 20221130131423.png]]

```sh
-c : Community String (Default: Public)
```


### Metasploit
use **auxiliary/scanner/snmp/snmp_login** OR **auxiliary/scanner/snmp/snmp_enum**![[Pasted image 20221130132215.png]]

![[Pasted image 20221129103835.png]]

### onesixtyone
Fast SNMP Scanner
```sh
# Enter all list of IPs & possible community strings or enter them individually with the same args
onesixtyone -c community.txt -i ip.txt
```

### snmpwalk
```sh
# Translate hex --> ASCII
snmpwalk -c public -v1 -t 10 192.168.50.151 -Oa

# Enumerate Windows users
snmpwalk -c public -v1 192.168.50.151 1.3.6.1.4.1.77.1.2.25

# Enumerate all process running
snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.25.4.2.1.2

# Enumerate Installed software
snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.25.6.3.1.2

# List all TCP ports listening
snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.6.13.1.3
```

| String ID              | Function         |
| ---------------------- | ---------------- |
| 1.3.6.1.2.1.25.1.6.0   | System Processes |
| 1.3.6.1.2.1.25.4.2.1.2 | Running Programs |
| 1.3.6.1.2.1.25.4.2.1.4 | Processes Path   |
| 1.3.6.1.2.1.25.2.3.1.4 | Storage Units    |
| 1.3.6.1.2.1.25.6.3.1.2 | Software Name    |
| 1.3.6.1.4.1.77.1.2.25  | User Accounts    |
| 1.3.6.1.2.1.6.13.1.3   | TCP Local Ports  |


### snmpbulkwalk
```sh
snmpbulkwalk -c [Community String] -v2c [IP] .
```

`-v2c` is the SNMP Version


### SNMP Bruteforce
#snmphydra 