![](https://miro.medium.com/v2/resize:fit:688/1*MlhClLonAr0cXQlVxouiwA.png)

## Challenge Description

> _“I don’t like scrolling down to read the code of my website, so I’ve squished it. As a bonus, my pages load faster!”_

## Initial Analysis

The challenge title and description reference code minification — a standard front-end optimization technique that removes unnecessary characters, whitespace, and line breaks to reduce payload sizes and improve page load speeds.

The challenge hint suggests viewing the raw source code via shortcuts like `CTRL+U` / `⌘+U`, prepending `view-source:` to the target URL, or using `curl` from the command line. This confirms that the target string is stored directly within the client-side markup.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*ksxdarbtevIg53u2u7AOaA.png)

## Exploitation Flow

## 1\. Viewing the Page Source

Opening the page in a browser displays a simple web layout. To inspect the raw HTML document behind it, I opened the source view using the shortcut:

Plaintext

```
CTRL + U (or right-click -> View Page Source)
```

As suggested by the challenge name, the entire HTML markup was compressed into a minified, single-line format.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*HDKAc6ePSzPI0eno5zpzow.png)

## 2\. Searching for the Flag

Instead of manually unminifying or formatting the code, I used the built-in browser search functionality (`CTRL + F`) to find the standard picoCTF flag format:

Plaintext

```
picoCTF{
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*ZqF0apA34u7z9yXf1ZCqog.png)

The search matched a hidden string embedded directly inside the `class` attribute of a `<p>` tag.

**Flag:** `picoCTF{pr3tty_c0d3_743d0f9b}`

## Conclusion & Key Takeaways

Minification and code compression are designed solely for performance optimization, not security or confidentiality.

- **Minified code is not hidden:** Minification strips whitespace and shortens variable names, but all static text, inline strings, and attribute values remain completely readable.
- **Keep secrets server-side:** Any data included in HTML attributes, class names, comments, or client-side scripts is accessible to anyone inspecting the page.
