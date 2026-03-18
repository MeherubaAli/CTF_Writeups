# Crack the hash
Cracking hashes challenges

### Level: Easy
### Room Overview: Crack Hashes

## Tools Used:
- CrackStation
- Hashcat
- John the Ripper
- Hashes.com

## Task 1 [Level 1]
Firstly, we try CrackStation where we can crack any hash type (if it's supported) for free
`48bb6e862e54f2a795ffc4e541caed4d` (MD5) <br>
Ans:`easy`

`CBFDAC6008F9CAB4083784CBD1874F76618D2A97` (sha1) <br>
Ans:`password123`

`1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032` (sha256) <br>
Ans:`letmein`

`$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom` <br>
Looks like this is a unrecognizable format for CrackStation.  Then we can try Hashcat. Let's search for `Hashcat examples` to see if there's any match. <br>There is and it looks like it is a `bcrypt` hash type and mode 3200 on HashCat. Now let's go to our Attackbox terminal which has all the tools installed. Now let's save the hash on our machine `nano hash.txt` and execute the command:`cat /usr/share/wordlists/rockyou.txt | grep -o -w ''\w\{4\}' > fourRock.txt`to shorten the wordlist (my lifespan isn't that long).`john --wordlist=fourRock.txt hash.txt` It look me less than 2 minutes after shortening the txt file and using John the Ripper. I wasted 25 minutes of my life on hashcat and the full rockyou.txt. <br>You win some & you learn some. <br>
Ans:`bleh` (sounds like ragebait to me)

We can try again with CrackStation with this one. <br>
`279412f945939ba78ce0758d3fd83daa` (MD4) <br>
Ans:`Eternity22`

## Tashk 2 (Level 2)
Let's try again with CrackStation. although the room says otherwise. <br>
`F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85` (sha256) <br>
Ans:`paule`

`1DFECA0C002AE40B8619ECF94819CC1B` (NTLM) <br>
Ans:`n63umy8lkf4i`

`Hash: $6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.` (sha512) <br>
`Salt: aReallyHardSalt` <br>
For this, we do the same with John (I'm not touching hashcat with a ten-foot pole). First shorten the rockyou.txt file to meet the input conditions. `cat /usr/share/wordlists/rockyou.txt | grep -o -w '\w\{6\}' > sixRock.txt`. This still takes some time. Then we save the hash text in a file called `six.txt` [WITHOUT the salt]. Because if you look closely, the salt is in the hash strings. then like before, we execute `john --wordlist=sixRock.txt six.txt`. If you want to try it out with Hashcat, the mode is 1800 (-m 1800). But it took me a little more than 8 minutes with John the Ripper. Best of luck with Hashcat :) <br>
Ans: `waka99` 

`Hash: e5d8870e5bdd26602cab8dbe07a942c8669e56d6` <br>
`Salt: tryhackme` <br>
This is interesting. It is HMAC-sha1. This time we have to use the salt which is supposed to make harder for others to crack passwords. Let's see...
It is (-m 1710) on Hashcat. I'm going to try both. <br>
Also, this time, we're going to use the whole rockyou.txt as the input conditions are longer than the others before. We have saved the hash in `salt.txt` file exactly like this: `e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme` (be careful of any whitespaces). Then we get to cracking (fingers crossed). I used these commands and some surroudnign these but nothing was working as the salt is a text `hashcat -m 160 lasthash.txt /usr/share/wordlists/rockyou.txt` & `john --wordlist=/usr/share/wordlists/rockyou.txt salt.txt`. Read this if you're struggling: https://www.openwall.com/lists/john-users/2020/11/27/1 - I had the same trouble. 
As a last resort, I used the web app tool called `hashes.com`, it is a lot like CrackStation, but it can crack the passwords with salt as well. And [FINALLY] found a match there. <br>
Ans: `481616481616`

Ganbare <3
