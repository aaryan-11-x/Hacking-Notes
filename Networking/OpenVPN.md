# Run the VPN connection as a daemon in the background
```sh
sudo openvpn --config [name.ovpn] --daemon
```

```shell
# Find the PID of the OpenVPN process
pid=$(sudo ps aux | grep -v grep | grep -i open* | awk -v FS=' ' '{print $2}')

# Send SIGTERM to the PID
sudo kill -9 $pid
```