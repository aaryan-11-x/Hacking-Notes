1) [[Content Discovery]]
2) [[Nikto]]

---
# Nmap
```sh
nmap -sV --script=http-enum -oA nibbles_nmap_http_enum 10.129.42.190
```

- Enumerates directories used by popular web applications and servers.
- This parses a fingerprint file that's similar in format to the [[Nikto]] Web application scanner. This script, however, takes it one step further by building in *advanced pattern matching* as well as having the ability to identify specific versions of Web applications.

---
# Dump Git directory from a Website
```sh
git-dumper [URL] source
```

---
