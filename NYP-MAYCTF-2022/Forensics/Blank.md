# **Blank**

### Challenge
To facilitate the vaccine distribution process, you save everyone’s details in a file. Today, you noticed the file is blank. Does it mean everybody is vaccinated? Or something is wrong with the blank file?

Note: Please dont email anyone for this challenge, it is not in scope

> File: [blank.jpg](files/blank.jpg)
***

**Solution**

We are given another jpeg file, lets check it out.

```bash
# file blank.jpg    
blank.jpg: Zip archive data, at least v2.0 to extract, compression method=store
```
Would you look at that, its a ZIP file!

```bash
# mv blank.jpg blank.zip

# unzip blank.zip    
Archive:  blank.zip
   creating: blank/
   creating: blank/.git/
   creating: blank/.git/branches/
 extracting: blank/.git/COMMIT_EDITMSG  
  inflating: blank/.git/config       
  inflating: blank/.git/description  
 extracting: blank/.git/HEAD         
   creating: blank/.git/hooks/
  inflating: blank/.git/hooks/applypatch-msg.sample  
  inflating: blank/.git/hooks/commit-msg.sample  
  inflating: blank/.git/hooks/fsmonitor-watchman.sample  
  inflating: blank/.git/hooks/post-update.sample  
  inflating: blank/.git/hooks/pre-applypatch.sample  
  inflating: blank/.git/hooks/pre-commit.sample  
  inflating: blank/.git/hooks/pre-merge-commit.sample  
  inflating: blank/.git/hooks/pre-push.sample  
  inflating: blank/.git/hooks/pre-rebase.sample  
  inflating: blank/.git/hooks/pre-receive.sample  
  inflating: blank/.git/hooks/prepare-commit-msg.sample  
  inflating: blank/.git/hooks/push-to-checkout.sample  
  inflating: blank/.git/hooks/update.sample  
 extracting: blank/.git/index        
   creating: blank/.git/info/
  inflating: blank/.git/info/exclude  
   creating: blank/.git/logs/
  inflating: blank/.git/logs/HEAD    
   creating: blank/.git/logs/refs/
   creating: blank/.git/logs/refs/heads/
  inflating: blank/.git/logs/refs/heads/master  
   creating: blank/.git/objects/
   creating: blank/.git/objects/47/
 extracting: blank/.git/objects/47/ee296d65cf5dffacf0a9feed1645c292d29348  
   creating: blank/.git/objects/4b/
 extracting: blank/.git/objects/4b/825dc642cb6eb9a060e54bf8d69288fbee4904  
   creating: blank/.git/objects/80/
 extracting: blank/.git/objects/80/6a177845583291d08280d1b9a7b98dbb0ac777  
   creating: blank/.git/objects/89/
 extracting: blank/.git/objects/89/923e751d22556f5b90f7f8186adeeb200a1ae3  
   creating: blank/.git/objects/94/
 extracting: blank/.git/objects/94/d17b8704995252145a14a6e92607689e9c20b7  
   creating: blank/.git/objects/info/
   creating: blank/.git/objects/pack/
   creating: blank/.git/refs/
   creating: blank/.git/refs/heads/
 extracting: blank/.git/refs/heads/master  
   creating: blank/.git/refs/tags/

```
Hm. Looks like its a git file from what I can see.

```bash
# cd blank/.git

# ls
branches  COMMIT_EDITMSG  config  description  HEAD  hooks  index  info  logs  objects  refs

```
Maybe there is useful information we can get in "COMMIT_EDITMSG".
```bash
# cat COMMIT_EDITMSG
Remove stuff
```
Remove stuff? Perhaps the flag is hidden in one of the commits.

```bash
# cd refs/heads/ 

# cat master        
89923e751d22556f5b90f7f8186adeeb200a1ae3

# git show 89923e751d22556f5b90f7f8186adeeb200a1ae3

commit 89923e751d22556f5b90f7f8186adeeb200a1ae3 (HEAD -> master)
Author: edwinc <realaccount@hotmail.com>
Date:   Fri May 6 16:16:56 2022 +0800

    Remove stuff

diff --git a/flag.txt b/flag.txt
deleted file mode 100644
index 94d17b8..0000000
--- a/flag.txt
+++ /dev/null
@@ -1 +0,0 @@
-NYP{h3y_g!t_15_fUn}
```

Found the flag! Looks like it was in the only commit.

> Flag: **NYP{h3y_g!t_15_fUn}**