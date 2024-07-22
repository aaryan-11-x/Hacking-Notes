```sh
ffuf -w /usr/share/seclists/Discovery/Web-Content/api/api-endpoints.txt -u http://192.168.197.16:5002/FUZZ -ic -c

# Change HTTP Request Methods
GET, POST, PUT, DELETE, OPTIONS, HEAD, PATCH, INVENTED

# Change Content-Type
Content-Type: application/json
Content-Type: application/xml
Content-Type: application/x-www-form-urlencoded
Content-Type: text/html
Content-Type: text/plain
Content-Type: text/xml
```

https://exploit-notes.hdks.org/exploit/web/api/