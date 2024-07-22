%% Steghide is a steganography program that is able to hide data in various kinds
of image- and audio-files. The color- respectivly sample-frequencies are not
changed thus making the embedding resistant against first-order statistical
tests. %%

#### Commands

The ***First Argument*** must be one of the following:
 - **embed**          embed data
 - **extract**         extract data
 - **info**            display information about a cover- or stego-file
		info [filename]       display information about [filename]
- **encinfo**     display a list of supported encryption algorithms


***Embedding*** Options:
- **-ef**        select file to be embedded
	   -ef [filename]        embed the file [filename]
-  **-cf**        select cover-file
	   -cf [filename]        embed into the file [filename]
 - **-p**, **--passphrase**        specify passphrase
	   -p [passphrase]       use [passphrase] to embed data
 - **-sf**, **--stegofile**        select stego file
	   -sf [filename]        write result to [filename] instead of cover-file


![[Pasted image 20230124103446.png]]

![[Pasted image 20230124104012.png]]