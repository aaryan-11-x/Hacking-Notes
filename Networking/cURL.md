```sh
# To download file with it's remote name & a progress bar
curl -# -0 inlanefreight.com/index.html

# Silent the status printed
curl -s -0 inlanefreight.com/index.html

# Write to file instead of stdout
curl -o [filename] inlanefreight.com

# Upload file to an endpoint
curl -F 'file=@shell.php' -F 'filename=@/home/alfredo/shell.php' http://192.168.45.234/endpoint

### For more info on upload files: https://medium.com/@petehouston/upload-files-with-curl-93064dcccc76


# Skip SSL Certificate check
curl -k https://inlanefreight.com

# Provide credentials with request (for Basic Auth)
curl -u [username:password] http://<SERVER_IP>:<PORT>/
OR
curl http://[username:password]@<SERVER_IP>:<PORT>/

# View full HTTP Request & Response (Like in BurpSuite)
curl -v inlanefreight.com
curl -vvv inlanefreight.com

# Display response headers & source code
curl -i inlanefreight.com

# Use proxy (with authentication)
curl -x proxyserver.com -u user:password inlanefreight.com
```

%% Note: HTTP version 1.X sends requests as clear-text, and uses a new-line character to separate different fields and different requests. HTTP version 2.X, on the other hand, sends requests as binary data in a dictionary form. %%

```sh
# Set User-Agent header
curl -A 'Mozilla/5.0' https://www.inlanefreight.com

# Set Custom Headers
curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/

# Set Mutiple Custom Headers
curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' -H 'Cookie: token=...' http://<SERVER_IP>:<PORT>/

# Set Cookie
curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/

# Send POST Request
curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/

# Send a HEAD request (Get only Response Headers)
curl -I inlanefreight.com

# To follow redirection
curl http://<SERVER_IP>:<PORT>/ -L
```

```sh
# Beautify JSON response
curl -s http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | jq

# Disable cURL path normalization (LFI)
curl http://192.168.120.155:3000/../../../../../../../../etc/passwd --path-as-is
```