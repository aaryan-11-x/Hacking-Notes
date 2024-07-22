# Method 1
- Look at the *header* to identify what *type of file* it is (HXD for Windows). 
- Compare the *header of the corrupt file to a non-corrupt file* (HXD for Windows). 
- Use **pngchecker (For PNG Images)** To Check For More *Errors*!
- Change the *bitmap width and height* **(For BMP Images)** (exiftool tool to identify the width and height, python hex function to *convert decimal to hex*, keep in mind that you might have to convert it from big to little endian or the other way around).
**<u>Example :-</u>
hex value : 0x348** ; **Enter Like This**    ![[Pasted image 20230302234901.png]]

- Resize the image if it appears distorted either *Online* or *Imagemagick*
```sh
convert warp_speed.jpg -crop 500x8 warp_left.jpg
```


# Method 2
- If *Two Images Are Given*, Combine Those Two Images In **Stegsolve**
- Select 1st Image, Then Go To *Image Combiner*, Then Select 2nd Image
![[Pasted image 20230306124203.png]]
- Choose From *Different Filters* And Find The Flag!



# Method 3
- For an *svg file*, Go To Inspect & Check For Any **Text Fields!**



# Method 4
- Check For **Additional Images** In The *Sources Section* Of The *Inspect Element Tab!*
- This Worked For The **Milkslap** picoCTF

# Method 5 - RGB Threshold
https://www.youtube.com/watch?v=9_BoWBC4EDM&list=PL1H1sBF1VAKVmrjF1uWh5wK9a2IzmUjPc&index=8&ab_channel=JohnHammond


##### Extract Text from Image
```sh
tesseract [img] stdout
```
