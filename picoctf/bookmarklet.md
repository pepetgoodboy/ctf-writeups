![](https://miro.medium.com/v2/resize:fit:700/1*8YxWwkzmC8KDzL3FwkLEGg.png)

## Challenge Description

> _“Why search for the flag when I can make a bookmarklet to print it for me?”_

## Hints:

- _“What happens when you click a bookmarklet?”_
- _“Web browsers have other ways to run JavaScript too.”_

## Initial Analysis

A **bookmarklet** is essentially a browser bookmark that contains a snippet of JavaScript code (using the `javascript:` pseudo-protocol) instead of a standard URL. When clicked, it executes that script directly within the context of the currently open webpage.

The challenge description and hints point out that we need to execute a client-side JavaScript snippet provided by the application to reveal the flag.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*TcE833vDfce8drgaFSFlmA.png)

## Exploitation Flow

## 1\. Extracting the JavaScript Code

Upon loading the challenge webpage, there is a prominent text area containing a JavaScript function designed to decode and display the flag.

Clicking the text area automatically copies the full JavaScript snippet to the clipboard.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*BRWw-_TKcpmHCJVjicLwWw.png)

## 2\. Executing the Script via Developer Tools

While you could create an actual browser bookmarklet with the code, the faster and more direct method suggested by the second hint (_“Web browsers have other ways to run JavaScript too”_) is to run it inside the browser’s developer console.

1.  Open Developer Tools (`F12` or `Ctrl + Shift + I`).
2.  Navigate to the **Console** tab.
3.  Paste the copied JavaScript code and hit **Enter**.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*1EpB_uv0AZ9RphJGmoZYfA.png)

## 3\. Retrieving the Flag

Executing the script instantly triggers an alert pop-up displaying the decrypted flag in the browser window:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*fg-yPD8ZADBLgOKdNGq07Q.png)

**Flag:** `picoCTF{p@g3_turn3r_e8b2d43b}`

## Key Takeaways

- **Client-Side Obfuscation & Logic:** Relying on client-side JavaScript to conceal sensitive values (even when encoded or obfuscated) is inherently insecure, as users have complete visibility and execution control over client scripts.
- **Console as an Execution Environment:** The browser console provides a full execution sandbox for JavaScript running against the current DOM context, making it an essential tool for rapid analysis during web assessments.
