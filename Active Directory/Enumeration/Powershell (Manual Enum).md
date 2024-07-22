Using our SSH terminal, we can upgrade it to a PowerShell terminal using the following command: `powershell`

```powershell
# Get full LDAP path script
$PDC = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner.Name
$DN = ([adsi]'').distinguishedName 
$LDAP = "LDAP://$PDC/$DN"
$LDAP

# Enumerate domain groups (local & global)
## Use LDAPSearch.ps1 in CTF/Tools/windows
. .\LDAPSearch.ps1

## User enumeration
LDAPSearch -LDAPQuery "(samAccountType=805306368)"

## Groups enumeration
foreach ($group in $(LDAPSearch -LDAPQuery "(objectCategory=group)")) {$group.properties | select {$_.cn}, {$_.member}}
$sales = LDAPSearch -LDAPQuery "(&(objectCategory=group)(cn=Sales Department))"
$sales.properties.member


## Requires Active Directory module installed
# To enumerate AD users
PS C:\> Get-ADUser -Identity gordon.stevens -Server za.tryhackme.com -Properties *

# To enumerate Groups
PS C:\> Get-ADGroup -Identity Administrators -Server za.tryhackme.com

# To enumerate group membership
PS C:\> Get-ADGroupMember -Identity Administrators -Server za.tryhackme.com

# Enumerate shared folders on a server
net view \\files01

# To enumerate AD objects that were changed after a specified date
PS C:\> $ChangeDate = New-Object DateTime(2022, 02, 28, 12, 00, 00)
PS C:\> Get-ADObject -Filter 'whenChanged -gt $ChangeDate' -includeDeletedObjects -Server za.tryhackme.com

# To get information about a specific domain
PS C:\> Get-ADDomain -Server za.tryhackme.com

# Force change the password of an AD user
PS C:\> Set-ADAccountPassword -Identity gordon.stevens -Server za.tryhackme.com -OldPassword (ConvertTo-SecureString -AsPlaintext "oldpwd" -force) -NewPassword (ConvertTo-SecureString -AsPlainText "newpwd" -Force)
```

-   `-Identity` - The account name that we are enumerating
-   `-Properties` - Which properties associated with the account will be shown, * will show all properties
-   `-Server` - Since we are not domain-joined, we have to use this parameter to point it to our domain controller
