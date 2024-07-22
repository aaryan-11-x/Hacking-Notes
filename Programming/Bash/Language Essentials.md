1) When you echo text in *single quotes*, it takes them literally *without any conversion*, but when you echo text in *double quotes*, it converts the variables if it's present.
```sh
a = "Aaryan"
echo 'My name is $a'
echo "My name is $a"
```

2) `read` command
`-r` is used to treat backslashes as *literal characters rather than as escape characters*
```sh
$ read -r input
Enter some text with a newline character: hello\nworld
$ echo "$input"
hello\nworld
```


3) `echo` flags
- `-n` : do not output the trailing newline


4) While Loop Syntax
```sh
#!/bin/bash

i=5
while (( $i >= 0 )) do
	echo "$i"
	((i--))
done
```


5) **Debugging**
```sh
# Executes the program line by line
bash -x ./file.sh

# To debug specific part of code
set -x
code....
set +x

# + if command executes Successfully!
# - if command doesn't execute Successfully
```