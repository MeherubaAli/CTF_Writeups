# A Rick and Morty CTF. Help turn Rick back into a human!

**Level:** Starters

### Room Overview
This room is about network scanning, directory busting, and some basic linux commands. 

### Tools Used
- nmap
- gobuster

## ***What is the first ingredient that Rick needs?*** 

Before anything, deployed the VM (free virtual machine that comes with THM) or you can use your own local terminal. Copied the IP & pinged it ```ping <IP>``` [It should be good]. 
- scan the IP using ```nmap -A <IP>``` [-A for an aggressive scan]. You can see that both ssh (22) & http (80) ports are open
- Went to the website or copied/pasted the IP address into the browser. Went to the page source which was an html file ```index.html``` . In the file, there were markdown comments which included Rick's computer's username ```R1ckRul3s```.
- Went back to the terminal. Since our http port is open, we can use gobuster (directory busting tool) to find out what directories/file/DNS servers are on Rick's computer using brute-force. Type in the command ```gobuster dir -u http://10.49.174.154/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html,js,css```. This command will brute-force into the directory and search the wordlists for those exact matches of the strings.
- Checked each of the url completely e.g., ```https://<IP>/robots.txt``` & ```https://<IP>/login.php``` 
- In the ```/robots.txt``` page, found the password ```Wubbalubbadubdub```
- In the ```/login.php``` page, found the login page & pasted in the pass & the username we found from html file previously.
*We're finally inside Rick's computer!*
- In the ```Commands``` page, typed in ```ls```. It listed out all the files in the current dir. Other than usual files, there is one strange looking ```txt``` file named ```Sup3rS3cretPickl3Ingred.txt```
**Usually, to read file content, ```cat [filename]``` is enough. But Rick being Rick, he disabled the feature.**
- Instead, typed ```less Sup3rS3cretPickl3Ingred.txt```
***VOILA!*** Found out our ingredients in Rick's potions aka the first flag on TryHackMe room.

## ***What is the second ingredient that Rick needs?***

- In the ```Commands``` page, typed in ```ls /home/```. Saw the users under home dir. One of them was ``rick``. 
- Typed in `ls /home/rick/`. There it was, second ingredients file.
- Next typed in `less /home/rick/"second ingredients"` to read the file and found the second ingredient.

## ***What is the third ingredient that Rick needs?***

First type in `sudo -l` to see sudo privileges. Apparently everything with sudo command and no password. 
- So type `sudo ls /root/`. Found a file called `3rd.txt`. It must be the third ingredient.
- Then typed `sudo less /root/"3rd.txt"`. It gave us the last ingredient of Rick's potion. 

Hopefully, Rick turns into a human again.

  ---
  N.B. For this CTF, it is recommended to use Reverse Shell Engineering. But I found it unsuitable for any noob leveler. So I skipped it <3
