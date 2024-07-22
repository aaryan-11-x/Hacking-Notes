https://regexr.com/
| Pattern                         | Meaning                                                                                                       |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| *                               | All filenames                                                                                                 |
| b*.txt                          | All filenames that begin with the character "b"                                                               |
| Data???                         | Any filename that begins with the characters "Data" followed by exactly 3 more characters                     |
| [abc]*                          | Any filename that begins with "a" or "b" or "c" followed by any other characters                              |
| `[[:upper:]]*`                  | Any filename that begins with an uppercase letter. This is an example of a character class.                   |
| `BACKUP.[[:digit:]][[:digit:]]` | This pattern matches any filename that begins with the characters "BACKUP." followed by exactly two numerals. |
| `* [![:lower:]]`                | Any filename that does not end with a lowercase letter.                                                       |
| {0,255}                         | Any number between 0 to 255                                                                                   |

# Grep Commands
```sh
# Search for a specific string only
cat file.txt | grep -Fx [string]

# Get line numbers too
cat file.txt | grep -n [string]

# Print lines which DO NOT MATCH the keyword
cat file.txt | grep -v [string]

# Perl Regex
## -o : Only match given regex, {1,5} : 1 to 5 digits, /open: Print this string after matching in all lines
grep -oP '\d{1,5}/open' nmap.scan
```


## Grep Important flags
```sh
-R : Recursive grep search for files inside the folder
-c : Lists an integer value as to how many times the pattern was found
-i : Ignore case
-l : List the filename instead of the pattern
-n : List the line numbers too
-v : Print all the lines which do not match the keyword
-e : Specify multiple regex patterns

# Q) What's the difference b/w -e & -E?
-e flag can be used to specify multiple patterns, with multiple use of -e flag( grep -e PATTERN1 -e PATTERN2 -e PATTERN3 file.txt), whereas, -E can be used to specify one single pattern(You can't use -E multiple times within a single grep statement).
```