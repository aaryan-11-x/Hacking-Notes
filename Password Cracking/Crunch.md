Crunch Is a ***Wordlist Generator*** 

#### Syntax

```sh
crunch [min_length] [max_length] -o [file_name]
```

**-o** : Save Output To *File*
**-f** : To Use Character Set (Stored In */usr/share/crunch*)
**-t** : Specifies a *Pattern*
**-l** : Use Special Character As "*String*"
**-p** : Used For Creating Wordlist With *Permutation*
**-q** : Read The *Specified File* For *Permutation*
**-c** : Splits File After The *Specified Number Of Lines*, Only Works With ***-o START***
**-b** : Splits File After The *Specified File Size*, Only Works With ***-o START***
**-d** : Limits the number of *Duplicate Characters*
**-i** : *Inverts* The Pattern


- For Generating Wordlist With ***Specified Keywords***
```sh
crunch [min] [max] [Specify Keywords] -o output.txt
```
![[Pasted image 20221223203002.png]]


- To Generate With ***Predefined Character Set***
	**crunch** [min]  [max] **-f** /usr/share/crunch/charset.lst [Character_Set]![[Pasted image 20221223204340.png]]


- To Generate a ***Pattern Specific Wordlist***
	- @ - Lowercase
	- , - Uppercase
	- % - Numbers
	- ^ - Symbol

	Ex: **crunch** [min]  [max] **-t** [Pattern] ![[Pasted image 20221223224446.png]]

	- If You Know That A ***Special Character Occurs In Between The Password***
		Ex: **crunch** [min]  [max] **-t** [Pattern] **-l** [Any Pattern Of That Length With That Special Character]
![[Pasted image 20221223232203.png]]

- To Handle *Frequency Of Characters*
	Ex: **crunch** [min]  [max]  [Type Of Characters] **-d** 2[Pattern Like @%,^]
	![[Pasted image 20221224000629.png]]

- To Generate With ***Permutation Of Words***
	Ex: **crunch** [min] [max] -p [Words With Spaces] (**Note: Min Max Values Will Be Ignored But Still give them for Syntax**)
	![[Pasted image 20221223233440.png]]


- For Pattern Inversion
![[Pasted image 20221224001020.png]]