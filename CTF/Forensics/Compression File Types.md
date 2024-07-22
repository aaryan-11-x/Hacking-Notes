### Reverse Hex Dump
**Generate a file** from hexdump
```sh
xxd -r [file] > [new_file]
```

##### gzip
- A format for *compressing single files*. It is often used on Unix-based systems. It Compresses Files Upto ***90%***
- For **Decompression** :-
	gunzip [filename.gz]

#### bzip2
```sh
bzip2 -d [file.bz2]
# OR
bunzip [file.bz2]
```

##### lz4
- A compression algorithm designed for *high-speed compression and decompression*
- For **Decompression** :-
	unlz4 [filename.lz4]


##### lzma
- A compression algorithm used in *7Z and XZ formats*.
- For **Decompression** :-
	unxz [file.lzma]


##### lzop
- A Lossless data compression algorithm that is used to *compress files to save space on storage devices or to reduce transmission times* over the internet. It is designed to be very fast, making it useful in situations where *speed is a priority*.
- For **Decompression** :-
	lzop -d [file.lzo]


###### lzip
- Lzip is a lossless file compressor that uses the LZMA algorithm for compression. It is similar to gzip or bzip2, but *offers better compression ratios* in many cases.
- For **Decompression** :-
	lzip -d [file.lz]


###### xz
- An XZ file is a compressed file format that uses the LZMA2 compression algorithm. It is used for *compressing large files or data sets.*
- For **Decompression** :-
	xz -d [filename.xz]
- To Extract a **Compressed Tarball** :-
	tar -xJf [filename.tar.xz]

