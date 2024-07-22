## Define Description of the Program
```python
import argparse

parser = argparse.ArgumentParser(description="Pwntools exploit for vsftpd 2.3.4", usage="%(prog)s --host [RHOST]", epilog="Example: python %(prog)s --host 127.0.0.1")

# usage: Define usage of program
# epilog: Give an example of how to run the program
```


## Add Arguments
```python
parser.add_argument("--host", help="Define the Target Server IP", dest="host", required=True)
parser.add_argument("-p", "--port", help="Define the port number (Default 21)", metavar="Port Number", dest="port", default=21)

# Parse all arguments
args = parser.parse_args()
host = args.host
port = args.port
```

1) `nargs` (Stores args in a list)
	- int
	- ? (Give no arguments or atleast 1)
	- * (Give 0 or many args)
	- + (Give 1 or many args)

For Explanation of all arguments, visit https://docs.python.org/3/library/argparse.html

