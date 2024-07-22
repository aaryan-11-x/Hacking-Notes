```sh
TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/var/lib/tftpboot"
TFTP_ADDRESS="0.0.0.0:69"

nmap -n -Pn -sU -p69 -sV --script tftp-enum <IP>
```

```sh
# Connect to a TFTP server
tftp [IP]

# Check the status
tftp> status
Connected to 192.168.0.103.
Mode: netascii Verbose: off Tracing: off Literal: off
Rexmt-interval: 5 seconds, Max-timeout: 25 seconds

# Enable verbose mode
tftp> verbose
Verbose mode on.

# Download & Upload files
tftp> get [file]
tftp> put [file]
```
