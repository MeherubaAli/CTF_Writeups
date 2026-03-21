# [Missing Person](https://tryhackme.com/room/missingperson)
Use your OSINT skills to help the police track down a missing person.

### Level: Easy
### Room Overview: 
Use Osint skills to find clues about the missing friend's whereabouts up until the moment he went missing.

## Tools used:
- Google Maps
- the Internet
- `exiftool` (optional)

First task is to download the task files, they are two pictures.

1. `What is the commercial name of this circuit?` <br>
From one of the pictures, we can see the name and if we search online maps, it gives us the name. <br>
Ans: `Pertamina Mandalika International Street Circuit`

2. `When did the event take place?` <br>
`Format: DD-DD/MM/YYYY.` <br>
The year the friend joined the MotoGP and then went missing is 2025. So we can search online about Petramina MotoGP 2025 and then find the date from the official website. <br>
Ans: `03-05/10/2025`

3. `He told me he ate delicious Mexican food. What is the name of the restaurant?` <br>
Open and then zoom in to the other picture, `food.jpg`. You can see the name of the restaurant on the tablecloth (rotate if you can't figure out). <br>
Ans: `Cantina mexicana`

4. `At what time was this photo taken?` <br>
`Format: HH:MM:SS.` <br>
Open the details of the picture. If there's metadata already listed like Linux does, you're fine, you can view the timestamp and date from there. <br> If you're can't, use the command; `exiftool food.jpg` on the terminal. <br>
Ans: 19:55:30

He sent me a message, this is the last I heard from him: ”Went to this cool MotoGP after party, and became friends with one of the local DJs who played that night. We’re going to visit a cave tomorrow.”


5. `What is the full address of the bar’s location?` <br>
At this point, just open the google maps, search bars near Petramina Mandalika and try to guess the answers by putting them in the answer input. Lucky there's only a couple of them. <br>
Ans: `Jl. Raya Kuta, Kuta, Kec. Pujut, Kabupaten Lombok Tengah, Nusa Tenggara Bar`

6. `What is the DJ's stage name?` <br>
Go to the Surfers' Bar social media, facebook or instagram - either is fine. Look for the post around September/October, 2025 and found a post about the MotoGP afterparty. In the reel, the DJ's stage name is mentioned. <br>
Ans: `BONG LELEH`

7. `After digging into the DJ's other online accounts, what cave does he take tourists to?` <br>
Just search for caves near Petramina and input. There aren't many. But one is special. <br>
Ans: `Gua Sumur`

8. `What number did the DJ list for his tour business?` <br>
`Format: Full number, no country code.`<br>
This is a fun one. Last question, I mentioned one was special. This is why. After stalkin- Oops! Investigating BONG LELEH's other social, Instagram, I found that he has several other businesses, for example a tour business in Gua Sumur. So I searched facebook again for Gua Sumur as it's basically half a graveyard and I found two accounts related to the Cave Tour business. One of them has the phone number the question is asking for. Without the country code, the asnwer is -
Ans: `85333137345`

Done! It was such an easy & fun room. Not so fun for the missing person though. But now we can find him, hopefully doing some legal investigation work. 

Also, on behalf of the whole TryHackMe community, sincerest apologies to Bong Leleh. He is a real person whose life we stalked *Side eye*

Maaf dan Salamat Po!
