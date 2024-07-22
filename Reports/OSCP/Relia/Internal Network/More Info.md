Performed AS-REP Roasting with jim's creds & got michelle's hash
Performed Kerberoasting with jim's creds & got iis_service hash for webby

```domainusers
Administrator:vau!XCKjNQBv2$
andrea:PasswordPassword_6
anna:f79bec80e693e632f973d32b3489af18
brad:970ba7d4c92f712d0363706d6144c058
dan:4b22394fc907bd7a74d1af6cc9aca348
iis_service:bb4136aaa06fe1688b300e2f9243e85b
internaladmin:65a883e27cc4714738dfe4dce95001db
jenny:5ef6ddc308ac24d5423c0b983eee159c
jim:Castello1!
larry:47995d3e82d8e698f9b1a9d78c90aa7e
maildmz
michelle:NotMyPassword0k?
milana: 2237ff5905ec2fd9ebbdfa3a14d1b2b6
mountuser:DRtajyCwcbWvH/9
```

# User Important Info
michelle is a member of INTERNALRDP, so can login as RDP
andrea is a member of WKRDP

~~FILES has some shares, try with smbclient~~
~~Try Adrian's hash & zachary's hash~~

##### Shared Folders
```lua
IPC$       2147483651 Remote IPC          DC02.relia.com
NETLOGON            0 Logon server share  DC02.relia.com
SYSVOL              0 Logon server share  DC02.relia.com
ADMIN$     2147483648 Remote Admin        MAIL.relia.com
C$         2147483648 Default share       MAIL.relia.com
IPC$       2147483651 Remote IPC          MAIL.relia.com
ADMIN$     2147483648 Remote Admin        login.relia.com
C$         2147483648 Default share       login.relia.com
IPC$       2147483651 Remote IPC          login.relia.com
ADMIN$     2147483648 Remote Admin        WK01.relia.com
C$         2147483648 Default share       WK01.relia.com
IPC$       2147483651 Remote IPC          WK01.relia.com
ADMIN$     2147483648 Remote Admin        WK02.relia.com
C$         2147483648 Default share       WK02.relia.com
IPC$       2147483651 Remote IPC          WK02.relia.com
ADMIN$     2147483648 Remote Admin        INTRANET.relia.com
C$         2147483648 Default share       INTRANET.relia.com
IPC$       2147483651 Remote IPC          INTRANET.relia.com
ADMIN$     2147483648 Remote Admin        FILES.relia.com
apps                0                     FILES.relia.com
C$         2147483648 Default share       FILES.relia.com
IPC$       2147483651 Remote IPC          FILES.relia.com
monitoring          0                     FILES.relia.com
scripts             0                     FILES.relia.com
ADMIN$     2147483648 Remote Admin        WEBBY.relia.com
C$         2147483648 Default share       WEBBY.relia.com
IPC$       2147483651 Remote IPC          WEBBY.relia.com
```

# Location of our Remote Kali
```
\\tsclient\\kali-share
```