1) To *Seperate Words* With *Delimiters* & *Get The Exact Position* From The Delimited Words :-
```shell
cut -d [delimiter keyword] -f2,3,4
```
	![[Pasted image 20220619172431.png]]


2) To Replace Characters, Use Translate [tr] :-
```shell
tr [Old Character]  [Replacement Character]
```
	![[Pasted image 20220619173839.png]]

```sh
tr -d '\n'
tr -s a-z A-Z           # De-duplicate repeated characters & replace with uppercase

# -d : Delete characters
# -s : Squeeze repeated characters (de-duplicate the letter)
# -t : Concat source set with destination set
# -c : If you specify -c with -d to delete a set of characters then it will delete the rest of the characters leaving the source set which we specified (c stands for complement; as in doing reverse of something)

cat file.txt | tr -s '[:lower:]' '[:upper:]'
# [:alnum:]       all letters and digits
# [:alpha:]       all letters
# [:blank:]       all horizontal whitespace
# [:cntrl:]       all control characters
# [:digit:]       all digits
# [:graph:]       all printable characters, not including space
# [:lower:]       all lower case letters
# [:print:]       all printable characters, including space
# [:punct:]       all punctuation characters
# [:space:]       all horizontal or vertical whitespace
# [:upper:]       all upper case letters
# [:xdigit:]      all hexadecimal digits
```


3) In Terminal :-
`CTRL + SHIFT + F` to find string inside the terminal

4) To sort files
```sh
sort file.txt | uniq

# To also print the count of each unique line & ignore case
sort file.txt | uniq -c -i 

# To print lines that occured only once in the file
sort file.txt | uniq -u

# To print lines that occured more than once in the file
sort file.txt | uniq -d

# Sort results on the basis of counts
cat file.txt | sort | uniq -c | sort -n

# Sort results on the basis of counts in Descending Order
cat file.txt | sort | uniq -c | sort -nr
```


5) `jq` Command
```sh
# To print the value of a key
echo data.txt | jq '.age'
31
```