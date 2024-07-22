Developed by Mark Russinovich & part of *SysInternals* suite.

Pre-requisites to use this tool:
- The *user that authenticates* to the target machine needs to be *part of the Administrators local group*
- *ADMIN$* share must be available (Default)
- File and Printer Sharing has to be turned on (Default)

```cmd
# Get nt authority\system on the local machine
PsExec.exe -i -r cmd.exe

# Get a shell as another user on another machine
PsExec64.exe -i \\FILES04 -u corp\jen -p Nexus123! cmd

# If you have TGT of a user on another computer in the domain (klist)
PsExec64.exe \\172.16.0.1 cmd
```

![[Pasted image 20240626111551.png]]