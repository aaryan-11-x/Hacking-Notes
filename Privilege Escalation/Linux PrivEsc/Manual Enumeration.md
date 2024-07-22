# Commands
1) **hostname**
The `hostname` command will return the hostname of the target machine. *Although this value can easily be changed or have a relatively meaningless string* (e.g. Ubuntu-3487340239), *in some cases, it can provide information about the target system’s role within the corporate network* (e.g. SQL-PROD-01 for a production SQL server).

2) **uname -a**
Will print system information giving us additional detail about the kernel used by the system. This will be useful when searching for any potential kernel vulnerabilities that could lead to privilege escalation.
```sh
grep -qP '^flags\s*:.*\blm\b' /proc/cpuinfo && echo 64-bit || echo 32-bit
```

3) **/proc/version**
Looking at `/proc/version` may give you *information on the kernel version* and additional data such as *whether a compiler (e.g. GCC) is installed*.

4) **/etc/issue**
This file usually *contains some information about the operating system but can easily be customized or changed*. While on the subject, any file containing system information can be customized or changed.

5) **ps aux**
An effective way to see the *running processes* on a Linux system.

The “ps” command provides a few useful options.
-   `ps -A`: View all running processes
-   `ps axjf`: View process tree
![[Pasted image 20230519102031.png]]
- `ps -eo user,command` : Shows all the processes & which user is running it

If `ps` is *not available*, check `/proc/net/tcp` which *ports are open*
%% 0100007F: 127.0.0.1 & 00000000: 0.0.0.0 %%
```sh
cat /proc/net/tcp
  sl  local_address rem_address   st tx_queue rx_queue tr tm->when retrnsmt   uid  timeout inode
   0: 0100007F:0019 00000000:0000 0A 00000000:00000000 00:00000000 00000000     0        0 14687 1 f62f9740 100 0 0 10 20
   1: 00000000:0050 00000000:0000 0A 00000000:00000000 00:00000000 00000000     0        0 13788 1 f62f8040 100 0 0 10 0
   2: 00000000:0016 00000000:0000 0A 00000000:00000000 00:00000000 00000000     0        0 14640 1 f62f8bc0 100 0 0 10 0
   3: 9201A8C0:A99C 8E01A8C0:0539 01 00000000:00000000 00:00000000 00000000    33        0 15248 3 f62f9180 20 4 31 10 -1

echo $((16#0019))
25
echo $((16#0050))
80
echo $((16#0016))
22
```

6) **env**
The `env` command will show *environmental variables*.
The *PATH variable* may have a *compiler or a scripting language* (e.g. Python) that could be used to *run code on the target system or leveraged for privilege escalation*.
![[Pasted image 20230519102125.png]]

**alias** for checking if some command has been changed!

7) **sudo -l**
The `sudo -l` command can be used to list all commands your user can run using `sudo`

8) **id**
The `id` command will provide a general overview of the user’s privilege level and group memberships
![[Pasted image 20230519102507.png]]

9) **/etc/passwd**
Use `getent passwd | cut -d ":" -f 1` to get only the name of users on the system.
```sh
# Show users which can be logged in
grep -v -E "nologin|false|sync" /etc/passwd
```

10) **history** OR `cat ~/.*history | less`

11) **ip route**
To see which network routes exist.

12) **netstat -ano**
-   `-a`: Display all sockets
-   `-n`: Do not resolve names
-   `-o`: Display timers

13) **ss -tlnpu**
- `ss` : Query socket statistics
- `-t` : Only TCP sockets should be displayed
- `-l` : Only sockets in listening state should be displayed
- `-n` : Avoid hostname resolution
- `-p` : List process name


14) **find**
-   `find . -iname flag1.txt`: find the file named “flag1.txt” in the current directory
-   `find /home -iname flag1.txt`: find the file names “flag1.txt” in the /home directory
-   `find / -type d -name config`: find the directory named config under “/”
-   `find / -type f -perm 0777`: find files with the 777 permissions (files readable, writable, and executable by all users)
-   `find / -perm a=x`: find executable files
-   `find /home -user frank 2>/dev/null`: find all files for user “frank” under “/home”
-   `find / -mtime 10`: find files that were modified in the last 10 days
-   `find / -atime 10`: find files that were accessed in the last 10 day
-   `find / -cmin -60`: find files changed within the last hour (60 minutes)
-   `find / -amin -60`: find files accesses within the last hour (60 minutes)
-   `find / -size 50M`: find files with a 50 MB size
-   `find / -size +100M -type f 2>/dev/null` : find files larger than 100 MB size & redirect errors to /dev/null & have a cleaner output

16) **ls -al**
https://ostechnix.com/different-ways-to-list-directory-contents-without-using-ls-command/

17) `systemctl list-timers`
List all timed services by *systemd*. It's another method of setting cron jobs except using crontab.
```sh
# Run the command every second to update the time left
watch -n 1 'systemctl list-timers'
```


###### Below 3 Commands do the same thing :-
-   `find / -writable -type d 2>/dev/null` : Find world-writeable folders
-   `find / -perm -222 -type d 2>/dev/null`: Find world-writeable folders (Also works in FreeBSD)
-   `find / -perm -o w -type d 2>/dev/null`: Find world-writeable folders

-   `find / -perm -o x -type d 2>/dev/null` : Find world-executable folders
-   `find . -type f -executable -perm /a=x` : Find Files Executable by Everyone


##### Find development tools and supported languages
-   `find / -name perl*`
-   `find / -name python*`
-   `find / -name gcc*`


15) **Exposed Credentials**
- Check `bash_history` for potential password leaks
- Check log files, config files, ovpn files etc.
```sh
find / -name *conf* -type f 2>/dev/null | grep php
```
- Check for `.gpg` & `.asc` (PGP private key) which you might need to bruteforce


16) Check `/var/mails` if you find any **mail files**
	- They are in hex, convert to decimal
```
echo $((16#[0019]))
```


17) Check **SSH Config file** to identify which users are permitted for `SSH`
```sh
cat /etc/ssh/sshd_config
```

![[Pasted image 20231123173340.png]]

18) Check IP Tables file
```sh
cat /etc/iptables/rules.v4

# Generated by xtables-save v1.8.2 on Thu Aug 18 12:53:22 2022
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -p tcp -m tcp --dport 1999 -j ACCEPT      # *Non-Default configuration*
COMMIT
# Completed on Thu Aug 18 12:53:22 2022
```

19) Load kernel modules & information
```sh
# Load kernel modules
lsmod

Module                  Size  Used by
binfmt_misc            20480  1
rfkill                 28672  1
sb_edac                24576  0
crct10dif_pclmul       16384  0
crc32_pclmul           16384  0
ghash_clmulni_intel    16384  0
vmw_balloon            20480  0
...
drm                   495616  5 vmwgfx,drm_kms_helper,ttm
libata                270336  2 ata_piix,ata_generic
vmw_pvscsi             28672  2
scsi_mod              249856  5 vmw_pvscsi,sd_mod,libata,sg,sr_mod
i2c_piix4              24576  0
button                 20480  0

# Get additional info about a module
/sbin/modinfo libata

filename:       /lib/modules/4.19.0-21-amd64/kernel/drivers/ata/libata.ko
version:        3.00
license:        GPL
description:    Library module for ATA devices
author:         Jeff Garzik
srcversion:     00E4F01BB3AA2AAF98137BF
depends:        scsi_mod
retpoline:      Y
intree:         Y
name:           libata
vermagic:       4.19.0-21-amd64 SMP mod_unload modversions
sig_id:         PKCS#7
signer:         Debian Secure Boot CA
sig_key:        4B:6E:F5:AB:CA:66:98:25:17:8E:05:2C:84:66:7C:CB:C0:53:1F:8C
```

---
# Vulnerable Sudo Version
[[Sudo Priv Esc]]
