It's A similiar Tool Like [[OWASP Dirbuster]] Except It's Much Faster!

Use `seclists/Usernames/Names/names.txt` for *username enumeration*.

Firstly, 
```sh
apt install ffuf
```

Command :-
```sh
# For fuzzing "_files"
ffuf -w /usr/share/seclists/Discovery/Web-Content/big.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -ic -c

# Normal Bruteforce
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-big.txt:FUZZ -u http://10.10.43.2/FUZZ -ic -c
ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt:FUZZ -u http://10.10.43.2/FUZZ -ic -c

# Fuzzing Files (Most Common files of all extensions)
ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-large-files.txt -u http://10.10.43.2/FUZZ -ic -c
```

*-b : Coookie Data ("NAME1=VALUE1; NAME2=VALUE2")
-c : Colorized Output
-ic : Excludes Copyrighted lines
-w : Wordlist Path With Keyword FUZZ*
*-u : IP Address Of The Website With Keyword FUZZ*
*-o : Write output to file*
*-v : Verbose output, printing full URL and redirect location (if any) with the results. (default: false)*
*-r : Follow redirects
-of : Output file format (md, json. ejson, html, csv, ecsv)*

%% Use -of with -o %% 

---
# Extension Fuzzing
One common way to identify that is by finding the server type through the HTTP response headers and guessing the extension. For example, if the server is `apache`, then it may be `.php`, or if it was `IIS`, then it could be `.asp` or `.aspx`, or if `Java` then `.jsp` or `.do` and so on.

Also sometimes use `.pdf, .txt, .zip`

There is one file we can always find in most websites, which is `index.*`, so we will use it as our file and fuzz extensions on it.
```shell
ffuf -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://SERVER_IP:PORT/indexFUZZ -ic -c
```
%%  Note: The wordlist we chose already contains a dot (.), so we will not have to add the dot after "index" in our fuzzing.  %%

After getting the valid extension :-
```shell
ffuf -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php

# IIS Fuzzing
ffuf -w /usr/share/seclists/Discovery/Web-Content/iisfinal.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -ic -c
```

---
# Multiple Fuzzing
```sh
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ,/usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZ2 -u "http://192.168.1.105/admin/admin.php?FUZZ=FUZ2" -ic -c
```

---
# Recursive Fuzzing
In `ffuf`, we can enable recursive scanning with the `-recursion` flag, and we can specify the depth with the `-recursion-depth` flag. If we specify `-recursion-depth 1`, it will only fuzz the main directories and their direct sub-directories. If any sub-sub-directories are identified (like `/login/user`, it will not fuzz them for pages). The `-e` option specifies an extension to look for. In this case, it's set to `.php`, meaning it will try to find files with the `.php` extension.
`-v` to output the full URLs. Otherwise, it may be difficult to tell which `.php` file lies under which directory.

```shell
ffuf -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php,.php7 -v
```

---
# Sub-Domain Fuzzing
We can find wordlists in `/opt/useful/SecLists/Discovery/DNS/`
```sh
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://FUZZ.hackthebox.eu -ic
```

---
# Vhost Fuzzing
VHosts, short for **Virtual Hosts**, refers to a feature in web servers that allows hosting multiple websites on a single physical server. It enables the server to serve different websites with separate domain names or IP addresses, even though they are all hosted on the same machine.

When a web server receives a request from a client (e.g., a web browser), it uses the *Host header* in the HTTP request to *determine which website the client is trying to access*. With VHosts, the server can match the Host header to the configured virtual host settings and serve the appropriate website's content to the client.

Watch this video for better clarity : [Virtual Hosts Explained - YouTube](https://www.youtube.com/watch?v=iBjirLD5X7Q&ab_channel=myPHPnotes)

```shell
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://insanityhosting.vm:80/ -H 'Host: FUZZ.insanitiyhosting.vm' -ic -c
```
`-H` : Specify *host header*

---
# Filtering Results
```shell
# MATCH Options
-mc              Match HTTP status codes, or "all" for everything. (default: 200,204,301,302,307,401,403)
-ml              Match amount of lines in response
-mr              Match regexp
-ms              Match HTTP response size
-mw              Match amount of words in response

# FILTER Options
-fc              Filter HTTP status codes from response. Comma separated list of codes and ranges
-fl              Filter by amount of lines in response. Comma separated list of line counts and ranges
-fr              Filter regexp
-fs              Filter HTTP response size. Comma separated list of sizes and ranges
-fw              Filter by amount of words in response. Comma separated list of word counts and ranges
```

```shell
# Filter out responses with size 900
aaryan11x@htb[/htb]$ ffuf -w /usr/share/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs 900 -ic -c
```


---
# Fuzzing Parameters (GET & POST)
##### GET
```shell
ffuf -w /usr/share/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx

ffuf -w /usr/share/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=../../../../../etc/passwd -fs xxx
```

##### POST
```shell
ffuf -w /usr/share/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx

ffuf -w /usr/share/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=../../../../../../etc/passwd' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```

`-d` : POST data
`-X` : HTTP method to use
%% In PHP, "POST" data "content-type" can only accept "application/x-www-form-urlencoded". So, we can set that in "ffuf" with "-H 'Content-Type: application/x-www-form-urlencoded'" %%

---
# Proxy Traffic
Whether it's for [network pivoting](https://blog.raw.pm/en/state-of-the-art-of-network-pivoting-in-2019/) or for using [[Website Hacking/Tools/Burp Suite/Basics]] plugins you can send all the ffuf traffic through a web proxy (HTTP or SOCKS5).
```sh
ffuf -u http://10.10.237.155/ -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -x http://127.0.0.1:8080
```

It's also possible to send *only matches* to your proxy for replaying
```sh
ffuf -u http://10.10.237.155/ -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -replay-proxy http://127.0.0.1:8080
```

---
# Shortcut Commands
```sh
# Wordlist from Numbers 0 to 255
seq 0 255 | ffuf -u 'http://10.10.237.155/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
```
