# Basics
```python
from pwn import *

# Create a new process
# Recieve the output of the process
# Output is in bytes so decode it

io = process(['nmap', '127.0.0.1', '-sV'])
output = io.recvall()
print(output.decode())
```

---
# Handling Interactive Processes
```python
# PTY provides more interactive & TTY behaviour
io = process(['msfconsole', '-q'], stdin=PTY)
io.recvuntil(b'>')
io.sendline(b'use exploit/multi/handler')
io.sendline(b'set payload windows/x64/meterpreter/reverse_tcp')
io.sendline(b'set LPORT 4444')
io.sendline(b'set LHOST eth0')
# Interact with the process
io.interactive()
```

---
# RCE via SSH
```python
from pwn import *

s1 = ssh(host='192.168.1.1',user='dhruv',password='rathee')
c1 = s1.process('whoami')
print(c1.recvall().decode())
p1 = s1.shell("zsh")         # bash shell doesn't work properly, so use sh or zsh
p1.interactive()
```