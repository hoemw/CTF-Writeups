# **Seized**

### **Challenge**
We managed to extract this from our teacher's computer Can you find his secret files?

<br>
<br>

#### **Creator**: Gabriel Seet


#### **Category:** Forensics



<br>

---

### **Solution**
We are given two files, **brick.ad1** and **brick.ad1.txt**.

Given the file type, we can tell that it is an `image container file` and the challenge may have to do something with us opening the files to obtain the flag, using a `FTK imager`.

<br>

I'm using AccessData FTK imager for reference.

<br>
<br>

![alt-image](../img/evidenceerror.png)
An error popped up when I was adding the ad1 file into the software: `Image detection failed`.


It probably wasn't going to be this easy. I'm guessing either the file is corrupted or something else is at play here.

<br>

![alt-image](../img/brokenheader.png)
Analysing the file with a binary file editor, we can see that the header is all messed up.

<br>

For reference, this is what a normal .ad1 file header should be:
"`53 45 47 4D 45 4E 45 4E 54 44 46 49 4C 45`"

<br>
<br>

![alt-image](../img/fixedheader.png)
Using the hex editor (In this case I'm using hexedit), I fixed the file header.

<br>

![image](../img/ftkimager1.png)
Now I am able to open the .ad1 file and view the content. The flag.txt was a bait, and the actual flag is hidden in the jpg file.

<br>
<br>

![flag](../img/flag.jpg)
There she is!


Flag:
> **FLAG{1m4g3_1n_4n_1m4g3}**