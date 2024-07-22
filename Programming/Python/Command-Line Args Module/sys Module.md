- The **sys module** in Python provides various functions and variables that are used to manipulate different parts of the Python runtime environment. 
- It allows Operating on the Interpreter as it provides access to the variables and functions that Interact Strongly With The Interpreter.

![[Pasted image 20220621191103.png]]


1) **sys.argv**
This Command Allows To Take arguments from the command line.
%% Remember : The *First arg(0)* Will Always Be The *Name Of The Script Itself!* %%
![[Pasted image 20220621192047.png]]


2) `sys.stderr.write`
```python
if "-b64" not in sys.argv and "-b32" not in sys.argv:
	sys.stderr.write("Base Encoding not supported!")
```

3) `sys.path`
Used to play with the PYTHONPATH variable
```python
## Example (Present in D:\Python\Project\Modules); can also be a zip file
abc
 |__ def
      |__ mymodule.py


import sys
sys.path.append(r"D:\Python\Project\Modules")

# Then you can import modules from that package directory
import abc.def.mymodule
```
