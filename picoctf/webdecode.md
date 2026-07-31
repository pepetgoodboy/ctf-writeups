![](https://miro.medium.com/v2/resize:fit:700/1*_cZfKi-d_Y0J5BkZW-2g7g.png)

## Challenge Description

> _“Do you know how to use the web inspector?”_

## Initial Analysis

When navigating to the challenge website, the home page appears to be a standard, static web page with basic content. Given the description prompt mentioning the web inspector and hints hinting that data might be encoded, the objective was clearly centered around inspecting client-side source code for exposed sensitive information.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*GZoqDLtdLHj4JkoQt7vzpg.png)

## Exploitation Flow

## 1\. Source Code Inspection

I began by inspecting the main page elements using Developer Tools (F12) and checking the page source, but nothing out of the ordinary was present on the primary route.

Navigating through the site navigation, I opened the **About** page, which explicitly stated:

> “Try inspecting the page!! You might find it there”

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*dQKx9sC8VrSDjabNAiPREw.png)

## 2\. Locating the Hidden Payload

I right-clicked and selected **View Page Source** (or pressed `Ctrl + U`) to inspect the raw HTML of the About page.

While scanning the structure, I noticed a suspicious `notify_true` attribute inside a `<section>` tag containing an encoded string:

HTML

```
cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfZGYwZGE3Mjd9
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*zPbXjTORzPBjDDoQSukBnA.png)

## 3\. Identifying and Decoding the String

The padding and character set strongly pointed toward **Base64** encoding. To analyze and decode it, I passed the string into an online multi-decoder tool (such as [tools.redlimit.id](https://tools.redlimit.id/) using the Magic Detector under the Crypto section) or via the terminal:

Bash

```
echo "cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfZGYwZGE3Mjd9" | base64 -d
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*c9gnPYeSmYdPL8NERZOSDQ.png)

Decryption revealed the flag: `picoCTF{web_succ3ssfully_d3c0ded_df0da727}`

## Conclusion & Lessons Learned

This challenge highlights a fundamental web security principle: **Client-side code (HTML, CSS, JavaScript, metadata, and HTML attributes) is entirely public.**

- **Never hide secrets client-side:** Encoding (like Base64) is an obfuscation method, not encryption. It can be instantly reversed by anyone viewing the page source or inspecting DOM elements.
- **Remove Debug Metadata:** Sensitive data, environment flags, or staging comments should be thoroughly stripped before pushing code to production.
