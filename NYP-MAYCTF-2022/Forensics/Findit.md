# **Findit**

### Challenge
Oh no! You forgot where the survivor base is. Fortunately, you saved the message in an image. But now you can’t remember the passphrase…

***

**Solution**

We have yet another jpg file to work with.

```bash
# file nyp_info_steg.jpg                            
nyp_info_steg.jpg: JPEG image data, JFIF standard 1.01, resolution (DPI), density 96x96, segment length 16, baseline, precision 8, 1214x681, components 3

# binwalk nyp_info_steg.jpg                

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01

# stegseek nyp_info_steg.jpg /usr/share/wordlists/rockyou.txt 
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "123456"
[i] Original filename: "flag.txt".
[i] Extracting to "nyp_info_steg.jpg.out".
```
After abit of searching, I found that out there is a "flag.txt" hidden in the jpeg file.

```bash
# cat nyp_info_steg.jpg.out
NYP{Steg0hiD@rsuS}
```
Ayyyyyy-

Flag: **NYP{Steg0hiD@rsuS}**