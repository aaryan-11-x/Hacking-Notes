```sh
sudo apt install fail2ban -y
```

Once we have installed it, we can find the configuration file at `/etc/fail2ban/jail.conf`. We should make a backup if we make a mistake somewhere and it does not work as it should.
```sh
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.conf.bak
```

```sh
# /etc/fail2ban/jail.conf
...SNIP...

# [sshd]
enabled = true
bantime = 4w
maxretry = 3
```

With this, we enable monitoring for the SSH server, set the ban time to four weeks, and allow a maximum of 3 attempts. The advantage of this is that once we have configured our 2FA feature for SSH, fail2ban will ban the IP address that has entered the verification code incorrectly three times too. We should make the following configurations in the `/etc/ssh/sshd_config` file:
```sh
[cry0l1t3@VPS ~]$ sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
[cry0l1t3@VPS ~]$ sudo vim /etc/ssh/sshd_config
```

For More info, [Check this](https://academy.hackthebox.com/module/87/section/906)

---
# Disable Password Based Authentication
- Go to the `/etc/ssh/sshd_config file` and edit it. 
- Find the line that says `PasswordAuthentication yes` and change it to `PasswordAuthentication no` (remove the # sign and change yes to no). 
- Next, find the line that says `Include /etc/ssh/sshd_config.d/*.conf` and change it to `Include /etc/ssh/sshd_config.d/*.conf` (add a # sign at the beginning). 
- Save the file, then enter the command `sudo systemctl restart ssh`

```sh
egrep '^PasswordAuthentication|^#Include' /etc/ssh/sshd_config
#Include /etc/ssh/sshd_config.d/*.conf
PasswordAuthentication no
root@jenkins:~# systemctl restart ssh
```

