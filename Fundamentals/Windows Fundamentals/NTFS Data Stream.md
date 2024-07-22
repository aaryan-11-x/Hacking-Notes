![[Pasted image 20230122214400.png]]

![[Pasted image 20230122214424.png]]


#### Analogy
- To *Create a File & Hide It*
![[Pasted image 20230122220627.png]]

- To *Show The Hidden File*
	**notepad** [filename]  [hiddenfilename]
	![[Pasted image 20230122220844.png]]

- To *Bind .exe Into A txt File*
	![[Pasted image 20230122222138.png]]

- To Attach An *NTFS Data Stream* to a *Directory*
![[Pasted image 20230122222917.png]]


**NOTE** : From *Windows 7 Onwards*, You Can't Directly Execute an exe from command line unless you extract it first!

---
# Detection
```powershell
dir /R

# Once you find the ADS file
more < hm.txt:root.txt:$DATA
```