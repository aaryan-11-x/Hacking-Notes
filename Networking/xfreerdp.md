```shell
# Fullscreen
xfreerdp /d:za.tryhackme.com /u:'user.name' /p:'password' /v:10.200.56.288:3389 /drive:.,kali-share +clipboard -f /cert:ignore

# Fullscreen but control the window
xfreerdp /d:za.tryhackme.com /u:'user.name' /p:'password' /v:10.200.56.288:3389 /drive:.,kali-share +clipboard /workarea /smart-sizing /cert:ignore
```

- `/drive:.,kali-share` will let you mount the **current working directory** as a network drive in the RDP session, so you can upload and download files.
- `+clipboard` allows copying and pasting between the target.
- `/u` is your username, `/p` is the password.
- `/d` is the Domain name.
- `/v` is the Domain IP
- `-f` : Full-screen Mode

**Mounted device** on Target Machine
![[Pasted image 20230918171541.png]]

---
# Windows Usernames
If you get an email of the username `sg@lolbas.com`, there are high chances that the account name is `sg`!

---
# Get system Level Shell
In RDP sometimes you don't get `system` shell even though you are the Domain Admin. To solve this, use PsExec64.exe
https://youtu.be/uYDwvQw5gh4?feature=shared&t=638