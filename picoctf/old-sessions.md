![](https://miro.medium.com/v2/resize:fit:700/1*bN6eB7yCYRq-8PfXIoQHPg.png)

### **Challenge Description**

Proper session timeout controls are critical for securing user accounts. If a user logs in on a public or shared computer but doesn’t explicitly log out (instead simply closing the browser tab), and session expiration dates are misconfigured, the session may remain active indefinitely.

This then allows an attacker using the same browser later to access the user’s account without needing credentials, exploiting the fact that sessions never expire and remain authenticated.

Our goal is to analyze the website about **session management vulnerabilites** and retrieve the hidden flag.

### **Step 1 | Check the Website**

The challenge provides the following URL:

```
http://dolphin-cove.picoctf.net:54858/
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*dvR4Bwf-GoCty98LlcpUQw.png)

You must register new account before login.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*byinfrur_rchu4TvXdc5tw.png)

After register, you must login first and will be redirect to homepage like this.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*mUA_aU8bBJNm9b_6igqA0Q.png)

And if you check it, we got new clue in comments users.

```
Hey I found a strange page at /sessions
```

This immediately looks suspicious and worth investigating.

**Step 2 | Discovering the Session Endpoint**

Navigate to the hidden endpoint with change the url to:

```
http://dolphin-cove.picoctf.net:54858/sessions
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*yDONc7RBpxVjkmirWIPD2w.png)

And yeah, we find session admin and we can use it to the get flag.

**Step 3 | Change Session Admin**

After get session for admin, we must back to homepage and change the session.

To change the session, you can press ‘CTRL + Shift + I’, then go to tab ‘Applications’ and click ‘Cookies’. In the column Value double click and paste the session admin here and press ‘Enter’ to Save.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*K8900mFjrHdS6W8fSlPImA.png)

Finnaly we must refresh page to check if session is valid for admin or not.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*OW1DBMqKT6z9Aqh5mO3G_A.png)

And yeah, the session is valid and we success login as admin and the flag is “**picoCTF{s3t_s3ss10n_3xp1rat10n5_2766ccb8}**”

## **Key Takeaways**

This challenge highlights the importance of secure session management:

- Always implement **proper session expiration policies**
- Never expose **sensitive endpoint**
- Regenerate session tokens after login
- Implement **server‑side session validation**

Failing to do so can allow attackers to hijack accounts without ever needing login credentials.
