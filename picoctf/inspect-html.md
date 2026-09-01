![](https://miro.medium.com/v2/resize:fit:697/1*GQzPqLTVcpJsTgQBJ_HlSw.png)

## Challenge Description

> _“Can you get the flag?”_

## Hints:

- _“What is the web inspector in web browsers?”_

## Initial Analysis

When accessing the challenge website, the interface displays a standard static web page. The hint specifically references the browser’s built-in web inspector tools, indicating that the target data is directly exposed within the raw client-side markup.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*xUCB4pIi0otC7OTBnEAlqw.png)

## Exploitation Flow

## 1\. Viewing Page Source

To inspect the underlying HTML code behind the page layout, I opened the browser source view using:

Plaintext

```
CTRL + U (or right-click -> View Page Source)
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*ubGJha2HyvltTKZ8tgcgQQ.png)

## 2\. Retrieving the Flag

Scanning through the HTML document, often left inside an unstripped HTML comment (such as `<!-- picoCTF{...} -->`) or static markup elementimmediately revealed the plaintext flag:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*4BXHJ6UmUIM_SOGVoO713A.png)

**Flag:** `picoCTF{1n5p3t0r_0f_h7ml_fd5d57bd}`

## Vulnerability & Remediation

This challenge illustrates **Information Disclosure via Client-Side Comments / Markup (CWE-200 / CWE-615)**.

- **HTML Is Public:** Anything delivered to the browser (HTML structure, inline comments, element attributes, metadata) is fully visible to every end user.
- **Sanitize Production Code:** Always ensure sensitive information, developer notes, debug flags, or internal comments are stripped prior to deploying web applications.
