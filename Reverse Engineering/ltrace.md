The `ltrace` command can be used to *intercept and record the dynamic calls made to shared libraries*.
```sh
ltrace ./[executable_binary]
```

```sh
# Example
$ ltrace ./checker
getenv("admin")                                  = nil
puts("Not an Admin")                             = 13
Not an Admin
+++ exited (status 0) +++
```
