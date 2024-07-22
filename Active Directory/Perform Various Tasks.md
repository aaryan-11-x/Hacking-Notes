# Add Users
###### Method 1: GUI

![[Pasted image 20230615131331.png]]


##### Method 2: Powershell
```powershell
PS C:\htb> New-ADUser -Name "Orion Starchaser" -Accountpassword (ConvertTo-SecureString -AsPlainText (Read-Host "Enter a secure password") -Force ) -Enabled $true -OtherAttributes @{'title'="Analyst";'mail'="o.starchaser@inlanefreight.local"}
```

Check Out This [Webpage](https://blog.netwrix.com/2018/06/07/how-to-create-new-active-directory-users-with-powershell/) for more Info on **Adding users with Powershell**

---
# Delete a User
##### Method 1: GUI

![[Pasted image 20230615131715.png]]![[Pasted image 20230615131959.png]]![[Pasted image 20230615132012.png]]


##### Method 2: Powershell
```powershell
PS C:\htb> Remove-ADUser -Identity pvalencia

# To find an Enabled User
C:\PS> Get-ADUser -LDAPFilter '(!userAccountControl:1.2.840.113556.1.4.803:=2)' | findstr [name]

# To get all the Users in a Container
C:\PS> Get-ADUser -Filter * -SearchBase "OU=Finance,OU=UserAccounts,DC=FABRIKAM,DC=COM"
```


---
# Unlock a User
##### GUI

![[Pasted image 20230615132250.png]]
![[Pasted image 20230615132221.png]] 

##### Powershell
```powershell
# To Unlock a User
PS C:\htb> Unlock-ADAccount -Identity amasters

# To Rest a User Password
PS C:\htb> Set-ADAccountPassword -Identity 'amasters' -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "NewP@ssw0rdReset!" -Force)

# Force Password Change
PS C:\htb> Set-ADUser -Identity amasters -ChangePasswordAtLogon $true
```

---
# Create a New OU
##### GUI

![[Pasted image 20230615132644.png]]
![[Pasted image 20230615132616.png]]


##### Powershell
```powershell
PS C:\htb> New-ADOrganizationalUnit -Name "Security Analysts" -Path "OU=IT,OU=HQ-NYC,OU=Employees,OU=CORP,DC=INLANEFREIGHT,DC=LOCAL"
```

---
# Create a New Group
##### GUI

![[Pasted image 20230615132827.png]]![[Pasted image 20230615132835.png]]


##### Powershell
```powershell
PS C:\htb> Add-ADGroupMember -Identity analysts -Members ACepheus,OStarchaser,ACallisto
```


---
# Configure Group Policies
1. Go to **Group Policy Management**
2. Right-click the GPO we wish to modify and select "Edit". This will bring up the *Group Policy Configuration* Editor window.

Rest all Processes are Very Specific to the Task!


#### Enable/Disable Removable Storage
![[Pasted image 20230615134609.png]]



#### Prevent Access to Command Prompt
![[Pasted image 20230615134756.png]]



#### Set Password Policy
![[Pasted image 20230615134716.png]]


---

# Add a Logon Banner
- **Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies and select Security Options.**
- On the right pane of the console, select the *Interactive Logon: Message text for users attempting to logon* policy. This is used to specify the text message to be displayed to the users at the time of logon.
-  In the Security Policy Settings tab, check the *Define this policy* settings in the template checkbox. Enter the logon message to be displayed and click Apply and OK.  
- Next, select the *Interactive Logon: Message title for users attempting to login* policy. This is used to specify the title that appears on the title bar of the Interactive logon window.
- In the Security Policy Settings tab, check the *Define this policy settings in the template* checkbox. Enter an appropriate title and click Apply and OK. 
- After configuring the title and text of the interactive logon message, run the following command to apply the group policy.


---
# Join a computer to a domain

1.  On the Desktop, click the **Start** button, type **Control Panel**, and then press ENTER.
2.  Navigate to **System and Security**, and then click **System**.    
3.  Under **Computer name, domain, and workgroup settings**, click **Change settings**.
4.  Under the **Computer Name** tab, click **Change**.    
5.  Under **Member of**, click **Domain**, type the name of the domain that you wish this computer to join, and then click **OK**.

![[Pasted image 20230615162155.png]]
![[Pasted image 20230615162203.png]]![[Pasted image 20230615162217.png]]
![[Pasted image 20230615162230.png]]![[Pasted image 20230615162237.png]]
