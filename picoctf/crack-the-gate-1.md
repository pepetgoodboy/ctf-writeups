![](https://miro.medium.com/v2/resize:fit:691/1*YEFRsf6aRXDKgf76dZ6DTQ.png)

**Challenge Description**

We’re in the middle of an investigation. One of our persons of interest, ctf player, is believed to be hiding sensitive data inside a restricted web portal. We’ve uncovered the email address he uses to log in: `[ctf-player@picoctf.org](mailto:ctf-player@picoctf.org)`. Unfortunately, we don’t know the password, and the usual guessing techniques haven’t worked. But something feels off... it’s almost like the developer left a secret way in. Can you figure it out?

Our goal is to find the password of **_ctf-player@picoctf.org_** account.

**Step 1 | Check the Website**

The challenge provides the following URL:

```
http://amiable-citadel.picoctf.net:62710/
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*RhpwcLXbb0QGtOn1yn_e6Q.png)

The website is render simple login page.

In the challenge description, i have a clue “_it’s almost like the developer left a secret way in_”, it’s mean more clue leave in code because developer forget to delete in production.

**Step 2 | Try to View Page Source**

First i try to view page source to check if the password leave it.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*3QMFf0HzxbwZXlgB5UggpA.png)

I found a suspicious comment in source page.

If you notice, the plain text used a cryptography techniques, so i try to detect unknown encodings and decode them with my favorite site “[https://tools.redlimit.id/#/magic](https://tools.redlimit.id/#/magic)”.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*eAlQarBkaBcSpBikJ_bApQ.png)

I found more clue again “_NOTE: Jack — temporary bypass: use header “X-Dev-Access: yes”_”. It’s mean the website can bypass with manipulation headers.

**Step 3 | Try login & intercept the request**

Try login with email “ctf-player@picoctf.org” with random pass and intercept the request with burp suite

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*_Gcwv-TVXsjKSdqJ0BguAw.png)

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*iCXdcUoZivVxXWH2LymGoA.png)

Don’t forget send to repeater the requests.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*pu4rmaTzbajAHrFfqyyYfw.png)

**Step 4 | Add the headers “X-Dev-Access: yes”**

Add new headers “X-Dev-Access: yes” and send request again.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*_8wdJdJq5LuqnAKL9NtH9Q.png)

And boom, we got the flag.

```
picoCTF{brut4_f0rc4_7e5db33b}
```
