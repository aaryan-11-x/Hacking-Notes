#### Kill Processes
```sh
ss --tcp --udp --listen --numeric --process
kill -9 [PID]
```


#### Kill Process running on a port
```sh
fuser -k 8080/tcp
```
