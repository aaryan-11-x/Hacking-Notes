1) Normal QR Code
```sh
zbarimg [file]
```


2) **rMQR** Code
![[Pasted image 20231125231721.png]]

Use *QRQR* App from Play Store!![[Pasted image 20231125231820.jpg]]

---
## Merge QR Codes
```sh
# vertical sprite
convert image1.png image2.png image3.png -append result/result-sprite.png

# horizontal sprite
convert image1.png image2.png image3.png +append result/result-sprite.png
```
