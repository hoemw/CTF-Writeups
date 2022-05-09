# **What's Inside?**

### Challenge
"Are there any more survivors who are not vaccinated yet? You look at this image and hope to find some clues…"

> File: [challenge.jpg](files/challenge.jpg)

***

**Solution**

We have a jpeg file to work with, so I conducted the usual ctf forensics procedure for it.
```bash
# file challenge.jpg
challenge.jpg: JPEG image data, JFIF standard 1.01, resolution (DPI), density 120x120, segment length 16, baseline, precision 8, 557x561, components 3
```
```bash
# binwalk challenge.jpg 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01

```
```bash
# strings challenge.jpg | tail   
p=M|
Yimw-
d:#wG
W_Tllh
TveQ
Tv16
4Q^~
Fkts^(
C4Q\}~
TllQe3NpbXBsMy1zdDNnby05NDcxM30= 
```
Here, we see that there is a set of suspicious string at the tail of the file strings.

With the "=" at the back, I suspected that it was base64 encoded.

```bash
# echo "TllQe3NpbXBsMy1zdDNnby05NDcxM30=" | base64 --decode
```
> Flag: **NYP{simpl3-st3go-94713}** 