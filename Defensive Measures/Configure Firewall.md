```sh
# Status of Firewall
sudo ufw status

# Allow outgoing and deny incoming requests
sudo ufw default allow outgoing
sudo ufw default deny incoming

# Allow connection from specific ports
sudo ufw allow 22/tcp

# Deny connection from IP Addresses
sudo ufw deny from 192.168.100.25
# Deny connection on a specific interface
sudo ufw deny in on eth0 from 192.168.100.26

# Enable firewall
sudo ufw enable

# Reset default settings
sudo ufw reset
```
