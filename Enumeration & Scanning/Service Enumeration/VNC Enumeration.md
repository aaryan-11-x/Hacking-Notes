VNC usually uses ports `5800 or 5801 or 5900 or 5901`.

```sh
PORT    STATE SERVICE
5900/tcp open  vnc
```

## Enumeration
```sh
nmap -sV --script vnc-info,realvnc-auth-bypass,vnc-title -p <PORT> <IP>
msf> use auxiliary/scanner/vnc/vnc_none_auth
```

## Connect to vnc using Kali
```sh
vncviewer <IP>::[PORT]

# OR

vncviewer -passwd [3DES_passwd] IP::PORT     # if you have the 3DES encrypted password
```

## Decrypting VNC password
Default password is stored in: `~/.vnc/passwd`

If you have the VNC password and it *looks encrypted (a few bytes, like if it could be and encrypted password)*. It is probably ciphered with 3DES. You can get the clear text password using https://github.com/jeroennijhof/vncpwd

```sh
vncpwd <vnc password file>
```
You can do this because the password used inside 3des to encrypt the plain-text VNC passwords was reversed years ago.

https://book.hacktricks.xyz/network-services-pentesting/pentesting-vnc
https://www.hackingarticles.in/vnc-penetration-testing/
