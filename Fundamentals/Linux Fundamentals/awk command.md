> Developed by Aho, Weinberger, and Kernighan
> Awk is a scripting language used for manipulating data and generating reports.The awk command programming language requires no compiling, and allows the user to use variables, numeric functions, string functions, and logical operators.

```sh
# Execute commands from a script
awk -f script.awk input.txt

# Output result to a file
awk -o
```

![[Pasted image 20240619102537.png]]

```sh
# Search for a pattern inside a file
awk '/ctf/' file.txt
```
![[Pasted image 20240619103113.png]]

```sh
# Print field variables without spaces
awk '{print $1 $3}' file.txt

# Print with spaces in b/w (Default dimiliter: ' ')
awk '{print $1,$3}' file.txt

$0 prints the whole line
```

**NR (Number Record)**:  It is the variable that keeps count of the rows after each line's execution.
```sh
awk '{print NR,$0}' file.txt
```
![[Pasted image 20240619103511.png]]

**FS (Field Separator)**: It is the separator (default delimiter is ' ').
```sh
awk 'BEGIN{FS="o"} {print $2}' file.txt
# OR
awk -F o '{print $2}' file.txt
```

![[Pasted image 20240619104702.png]]

```sh
# Perform an action at the end
awk 'BEGIN {FS="o"} {print $1,$3} END{print "Total Rows=",NR}' file.txt
```
![[Pasted image 20240619104743.png]]

**RS (Record Seperator)**: Seperates rows with `\n` (Default)
```sh
awk 'BEGIN {RS="o"} {print $0}' file.txt
```
![[Pasted image 20240619104942.png]]

**OFS (Output Field Seperator)**: Similar to tr command
![[Pasted image 20240619105453.png]]![[Pasted image 20240619105505.png]]

**ORS (Output Record Seperator)**: Define output after each record is printed.
```sh
awk 'BEGIN{ORS=\n\n} {print $0}' file.txt
```
![[Pasted image 20240619105619.png]]