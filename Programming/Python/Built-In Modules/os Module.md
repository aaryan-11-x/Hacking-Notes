The OS module in Python provides functions for interacting with the Operating System.

1) os.cwd()
	###### Prints The Current Working Directory (CWD)
	![[Pasted image 20220621092021.png]]


2) os.chdir([Directory Name])
	###### Used To Change The CWD
	![[Pasted image 20220621092408.png]]


3) os.listdir()
	###### Prints The Folders Inside The CWD In A *List*
	![[Pasted image 20220621092935.png]]


4) os.mkdir([Folder Name])
	###### Creates A New Folder
	![[Pasted image 20220621093339.png]]

5) os.makedirs([Directory1/Directory2/...])
	###### Creates Directories Inside Directories
	![[Pasted image 20220621093921.png]]

6) os.rename([oldname], [newname])

7) os.path.join([path1], [path2])
	###### Joins **2 Paths** Without Worrying About The Slashes
	![[Pasted image 20220621095014.png]]


8) os.path.exists([pathname])
	###### Returns *True If The Path Exists* Or *False If It Doesn't*
![[Pasted image 20220621142103.png]]


9) os.system()
Run system commands
```python
# Returns 0 if command is successful
os.system("some_command < input_file | another_command > output_file")  
```


10) strerror
Gets the error message corresponding to the error code
```python
from os import strerror
print(strerror(i) for i in range(1,21))
Error code 1: Operation not permitted
Error code 2: No such file or directory
Error code 3: No such process
Error code 4: Interrupted system call
Error code 5: Input/output error
Error code 6: No such device or address
Error code 7: Argument list too long
Error code 8: Exec format error
Error code 9: Bad file descriptor
Error code 10: No child processes
Error code 11: Resource temporarily unavailable
Error code 12: Cannot allocate memory
Error code 13: Permission denied
Error code 14: Bad address
Error code 15: Block device required
Error code 16: Device or resource busy
Error code 17: File exists
Error code 18: Invalid cross-device link
Error code 19: No such device
Error code 20: Not a directory
```