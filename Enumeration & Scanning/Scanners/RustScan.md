- Faster than [[Nmap]]
- Automatically pipes ports into Nmap

# Multiple IP Scanning
```sh
rustscan -a 127.0.0.1,0.0.0.0
```

# Host Scanning
```sh
rustscan -a www.google.com, 127.0.0.1
Open 216.58.210.36:1
Open 216.58.210.36:80
Open 216.58.210.36:443
Open 127.0.0.1:53
Open 127.0.0.1:631
```

# CIDR support
```sh
rustscan -a 192.168.0.0/30
```

# Hosts file as input
```sh
# hosts.txt
192.168.0.1
192.168.0.2
google.com
192.168.0.0/30
127.0.0.1

rustscan -a "hosts.txt"
```

# Individual Port Scanning
```sh
rustscan -a 127.0.0.1 -p 53,80
```

# Ranges of ports
```sh
rustscan -a 127.0.0.1 --range 1-1000
```

# Scan with Nmap arguments
RustScan, at the moment, runs Nmap by default.
```sh
rustscan -a 127.0.0.1 -- -A -sC
```

# Random port Scanning
```sh
rustscan -a 127.0.0.1 --range 1-1000 --scan-order "Random"
```

# Quiet mode
```sh
rustscan -a 127.0.0.1 -q --range 1-10000
```

---
# ⚠️ WARNING
By default, RustScan scans 3000 ports per second.

This may cause damage to a server or make it very obvious you are scanning the server, thus triggering an unwelcome response like having your IP address blocked.

There are 2 ways to deal with this:

1.  Decrease batch size: `rustscan -b 10` will scan 10 ports at a time, each with a default timeout of 1000 (1 second). So, the maximum batch duration can be longer than the timeout: however long it takes to start (and finish processing) all the scans in the batch.
2.  Increase timeout: `rustscan -T 5000` means RustScan will wait for a response on a port for up to 5 seconds.