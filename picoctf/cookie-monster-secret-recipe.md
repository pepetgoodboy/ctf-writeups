![](https://miro.medium.com/v2/resize:fit:700/1*YXzShGHCyQuOzHdd1LqDfQ.png)

## Challenge Description

> _“Cookie Monster has hidden his top-secret cookie recipe somewhere on his website. As an aspiring cookie detective, your mission is to uncover this delectable secret. Can you outsmart Cookie Monster and find the hidden recipe?”_

## Initial Analysis

Upon accessing the target website, I was greeted with a standard login page. Given the challenge title and description referencing “Cookie Monster” and hidden recipes, the primary focus immediately pointed toward **HTTP Cookie manipulation/inspection**.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*myWAP2T3MtyFst6P4XB0dg.png)

Before interacting with the login form, I opened the browser Developer Tools and checked the **Application > Cookies** tab. At this initial stage, no session cookies were set.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*Gyj-JNNINDHwbASsTBCfjw.png)

## Exploitation Flow

## 1\. Triggering Cookie Generation

To see how the application handles authentication attempts, I submitted a dummy credential pair (`admin:admin`).

The application redirected to `login.php` and displayed an explicit clue:

> “Me no need password, Me just need cookies!”

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*d9Mip82sCN8GsQQQn_nC7g.png)

## 2\. Inspecting the Cookie Value

Re-checking the **Application > Cookies** tab revealed that a new cookie had been issued following the login attempt.

Inspecting its value, the character set and padding structure strongly suggested **Base64 encoding**.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*qVXViSq8uk2we625laW94g.png)

## 3\. Decoding the Payload

To reveal the hidden data stored inside the cookie string, I copied the Base64 value and decoded it directly in the Linux terminal:

Bash

```
echo "cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzXzczMTEwRUQxfQ==" | base64 -d
```

_(Alternatively, you can decode the string online using tools like_ [_base64decode.org_](https://www.base64decode.org/)_)._

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*OxRFRD7aTEigFtiywyiBuw.png)

The decoded string revealed the secret recipe containing the flag.

## Conclusion & Security Takeaways

Storing sensitive data such as plaintext secrets, access flags, or unencrypted session permissions directly inside cookies is a critical security vulnerability.

**Base64 is an encoding scheme, not encryption.** Anyone can easily decode Base64 strings.

To secure cookie-based session management:

- **Never store sensitive data client-side:** Keep secrets, flags, and authorization states on the server side.
- **Use Cryptographically Signed/Encrypted Cookies:** If state must be stored in cookies, sign them using strong cryptographic keys (e.g., HMAC) or encrypt them entirely.
- **Implement Proper Flags:** Always set the `HttpOnly` and `Secure` attributes on session cookies to mitigate XSS and transport-layer interception.
