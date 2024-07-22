**Powerview** is a powerful powershell script from powershell empire that can be used for enumerating a domain after you have already gained a shell in the system.

```cmd
powershell -ep bypass
```

Next, *Download* Powerview into the target and run it!
```powershell
. .\PowerView.ps1
```

---
# Powerview Cheatsheet
```powershell
# Get domain information
Get-NetDomain

# Get OS names of all computers
Get-NetComputer | select operatingsystem,dnshostname

# Enumerate domain users
Get-NetUser | select cn
Get-NetUser "username"

# Get User SPNs
Get-NetUser -SPN | select samaccountname,serviceprincipalname

# Enumerate a domain group
Get-NetGroup "Groupname"

# Enumerate Shared Folder
Invoke-ShareFinder

# Find shares in the domain
Find-DomainShare

# Get all the Operating Systems running in the network
Get-NetComputer -fulldata | select operatingsystem

# Determine if current user has admin access on any computer in the domain (Takes time to finish)
Find-LocalAdminAccess

# Check logged in users (Works on old Windows versions)
Get-NetSession -ComputerName files04 -Verbose

# Get ACL having GenericAll permission on a user/group
Get-ObjectAcl -Identity stephanie
Get-ObjectAcl -Identity "Management Department" | ? {$_.ActiveDirectoryRights -eq "GenericAll"} | select SecurityIdentifier,ActiveDirectoryRights

SecurityIdentifier                            ActiveDirectoryRights
------------------                            ---------------------
S-1-5-21-1987370270-658905905-1781884369-512             GenericAll
S-1-5-21-1987370270-658905905-1781884369-1104            GenericAll
S-1-5-32-548                                             GenericAll
S-1-5-18                                                 GenericAll
S-1-5-21-1987370270-658905905-1781884369-519             GenericAll

"S-1-5-21-1987370270-658905905-1781884369-512","S-1-5-21-1987370270-658905905-1781884369-1104","S-1-5-32-548","S-1-5-18","S-1-5-21-1987370270-658905905-1781884369-519" | Convert-SidToName
CORP\Domain Admins
CORP\stephanie
BUILTIN\Account Operators
Local System
CORP\Enterprise Admins
```

[PowerView CheatSheet - Undergrad CyberSec Notes (gitbook.io)](https://zflemingg1.gitbook.io/undergrad-tutorials/powerview/powerview-cheatsheet)
