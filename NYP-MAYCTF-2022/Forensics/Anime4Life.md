# **Anime4Life**

### Challenge
"Anime for life! Arigato!" One of the kids shouts when you are distributing the zombie virus vaccine. Think monkey. Good luck :D"
***

**Solution**

We were given a jpeg file, lets check it out.

```
# file anime.jpg
anime.jpg: JPEG image data, Exif standard: [TIFF image data, little-endian, direntries=13, height=533, bps=170, PhotometricIntepretation=RGB, orientation=upper-left, width=782], progressive, precision 8, 782x533, components 3
```
Nothing too interesting so far...


```
# binwalk anime.jpg          

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, EXIF standard
12            0xC             TIFF image data, little-endian offset of first image directory: 8
543954        0x84CD2         End of Zip archive, footer length: 22
```
Looks like there is a zip file embedded in the jpg. Lets extract it.

```
# mv anime.jpg anime.zip

# unzip anime.zip    
Archive:  anime.zip
warning [anime.zip]:  506732 extra bytes at beginning or within zipfile
  (attempting to process anyway)
file #1:  bad zipfile offset (local header sig):  506732
  (attempting to re-compensate)
file #1:  bad zipfile offset (local header sig):  506732

```
Ah right, the jpeg file is still there. I need to isolate the jpg from the zip file.

```
# xxd anime.zip | egrep "ffd9"
00002420: e017 5c44 df4e 3ddf ffd9 ffed 2bfc 5068  ..\D.N=.....+.Ph
00004fa0: fd54 782b c406 e017 5c44 df4e 3ddf ffd9  .Tx+....\D.N=...
0007bb60: f4ff 0087 f42e bfcf 675f ffd9 ff4b 0304  ........g_...K..
```
FFD9 is the Hex signature that marks the end of a jpg file. So the data after it will be the start of the ZIP file most likely.

<br>

So we will need to cut out the jpg data from this hexdump.

```
# xxd -s 0x7bb60 anime.zip
0007bb60: f4ff 0087 f42e bfcf 675f ffd9 ff4b 0304  ........g_...K..
0007bb70: 1400 0900 0800 7331 9854 3aed be96 c690  ......s1.T:.....
0007bb80: 0000 ff93 0000 0800 1c00 666c 6167 2e74  ..........flag.t
0007bb90: 7874 5554 0900 035a 2265 6293 2a65 6275  xtUT...Z"eb.*ebu
...
```
On closer inspection, the file signature of ZIP seems to be wrong, it being only ...K instead of PK.

I used a tool called HexEd.it, it makes editing hex values in a file a breeze.

```
# xxd anime.zip | head
00000000: 504b 0304 1400 0900 0800 7331 9854 3aed  PK........s1.T:.
00000010: be96 c690 0000 ff93 0000 0800 1c00 666c  ..............fl
00000020: 6167 2e74 7874 5554 0900 035a 2265 6293  ag.txtUT...Z"eb.
00000030: 2a65 6275 780b 0001 04e8 0300 0004 e803  *ebux...........
...
```
Now the file hex looks like this.

```
# unzip anime.zip 
Archive:  anime.zip
[anime.zip] flag.txt password:
```
We can use johntheripper for this.

```
# zip2john anime.zip > animehash   
ver 2.0 efh 5455 efh 7875 anime.zip/flag.txt PKZIP Encr: TS_chk, cmplen=37062, decmplen=37887, crc=96BEED3A ts=3173 cs=3173 type=8
```

```
# john --wordlist=/usr/share/wordlists/rockyou.txt animehash
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
Godspeed         (anime.zip/flag.txt)     
1g 0:00:00:00 DONE (2022-05-08 10:58) 25.00g/s 7168Kp/s 7168Kc/s 7168KC/s bedshaped..100370
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

```
# unzip anime.zip
Archive:  anime.zip
[anime.zip] flag.txt password: 
  inflating: flag.txt
```
We are almost there.

```
# file flag.txt 
flag.txt: PDF document, version 1.7 (zip deflate encoded)
```
On closer inspection, it looks like the ".txt" file is a pdf file.

```
# mv flag.txt flag.pdf
```

The flag is behind another password in the pdf file.

I realised that the password pattern is based on the anime character Killua Zoldyck from hunterxhunter. So I typed in "Killua" as the password and got the flag.

Flag: **NYP{M4g1c_byt35_4rE_fUn}**