# Love Letter Locker
Use your skills to access other users' letters.

### Level: Easy
### Link: https://tryhackme.com/room/lafb2026e2

Copied & pasted the MachineIP on the browser and we can access the web app. 
Registered and logged in.<br>
Wrote a letter.<br>
Opened it.<br.
if we look at the url, se can see that there's a number assigned to each letter. It's something like `<IP>/letter/3`
We can view other's letters by just changing the last digit in the url. If I go to `letter/2`. There's a sweet note. 
If I go to `/letter/1`, I found the flag.
That's it :) 

## Short Explanation:
What we just exploited are known as IDOR vulnerabilities. It is a type of access control vulnerability. It looks very simple, yet these are not uncommon when each pages or links aren't met with `IAAA standards` that allows user authentication, authorisation, etc.<br>
As per OWASP Top10 2025, it is part of A01: Broken Access Control which is at the top. So not very uncommon, no? :0

Ganbare <3
