# Corrupt Zip File
1) Change Header (and/or) Footer
2) strings

 
##### Run like a binary executable
- If it's a **Java file** :-
	![[Pasted image 20230729223647.png]]

```sh
# Forcefully fix the file
zip -FF 11BE2.zip -O archive-fixed.zip
```
This works for corrupt (or incompletely downloaded) files because the `jar` utility does not check for the `End-of-central-directory` signature before starting the extraction.
`.jar` files are zips internally.

```sh
# Unzip directly with 7z
7z x 11BE2.zip
```
