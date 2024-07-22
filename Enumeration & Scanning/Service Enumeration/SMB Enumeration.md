For Getting Info About The Target's NETBIOS, Use ***nbtstat*** On *Windows* & ***smbclient*** On *Linux* OR [[Nmap]] On *Both*.
This Command Is Used For Getting *NETBIOS Statistics* & *Current TCP/IP Connections*

---
# nbtstat
In ***Windows*** :-
nbtstat **-A** [IP Address]
**-a** : Input Remote Target's *Name*
**-A** : Input Remote Target's *IP Address*
![[Pasted image 20221127215011.png]]

---
# smbclient
```sh
smbclient -L [IP Address]

# Connect to the SMB Server with a username
smbclient -L //[IP] -U [username]

# Connect with username & password in one command (No spaces in -U)
smbclient -L //[IP] -U[username]%[password]
OR
smbclient -L //172.16.215.217/ -U hr_admin --password=Welcome1234

smbclient -U bob \\\\10.129.42.253\\users

smb: \> prompt off            # Turn off prompt for get
smb: \> timeout 100            # Increase timeout for downloading large files
smb: \bob\> get [filename]    # To download a file from smb server
smb: \> pwd                   # Check PWD (Go to HTTP & check if directory can be accessed!)
smb: \> put [filename]        # Put a Reverse Shell & execute from browser

# Pass-the-Hash with smbclient
smbclient \\\\192.168.50.212\\secrets -U Administrator --pw-nt-hash 7a38310ea6f0027ee955abed1762964b
```

**-I** : IP Address
**-L** : Lists shares
![[Pasted image 20221127215629.png]]

  
You can **recursively download the SMB share** too.
```sh
smbget -R smb://10.10.43.62//[sharename]

# To get shares for a specific user
smbget -R smb://10.10.43.62//[sharename] -U[username%password]
```

## Impacket-smbclient
Doesn't timeout so can use to download large files
```sh
impacket-smbclient 'resourced.local/V.Ventz:HotelCalifornia194!@192.168.213.175'

## Commands
shares              # To list all the shares
use Password Audit  # Connect to that share
get SYSTEM          # Download file
mget *              # Download all files in the directory
```

---
# smbmap
Tells the Share **Permissions** too unlike smbclient
```sh
smbmap -H [IP] -u [username] -p [password]
```

---
# Nmap
nmap **-p** 445 -A [IP]
OR
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.43.62
![[Pasted image 20221127230558.png]]


---
# Enum4Linux (Old)
```sh
enum4linux 10.17.36.8
```
[Enum4Linux Full Tutorial | Noob to Pro | 2023 (techyrick.com)](https://techyrick.com/enum4linux-full-tutorial/)

# Enum4linux-ng (New)
```sh
enum4linux-ng -A -R 10 [IP]
```
`-R`: RID Cycling for enumerating users

---
# SMB Password Bruteforcing
https://www.hackingarticles.in/password-crackingsmb/