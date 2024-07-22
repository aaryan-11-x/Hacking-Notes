1) `platform`
Lets you access the underlying platform's data, i.e., hardware, operating system, and interpreter version information.
```python
platform(aliased = False, terse = False)
```

<mark style="background: #FFF3A3A6;">aliased</mark> → when set to *True* (or any non-zero value) it may cause the function to present the alternative underlying layer names instead of the common ones;
<mark style="background: #FFF3A3A6;">terse</mark> → when set to *True* (or any non-zero value) it may convince the function to present a briefer form of the result (if possible)

```python
from platform import platform

# Example (Intel x86 + Windows ® Vista (32 bit))
Windows-Vista-6.0.6002-SP2
Windows-Vista-6.0.6002-SP2
Windows-Vista
```


2) `machine`
Know the architecture of the system
```python
from platform import machine

print(machine())
```

3) `processor`
Know the processor name

4) `system`
Print the OS name (Ex: Windows, Linux)

5) `version`
Get the OS version

6) `python_implementation()` → returns a string denoting the Python implementation (expect CPython here, unless you decide to use any non-canonical Python branch)
`python_version_tuple()` → returns a three-element tuple filled with:
- the major part of Python's version;
- the minor part;
- the patch level number.

```python
from platform import python_implementation, python_version_tuple

print(python_implementation())

for atr in python_version_tuple():
    print(atr)
```

Output
![[Pasted image 20240318002951.png]]

