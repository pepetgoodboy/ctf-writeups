![](https://miro.medium.com/v2/resize:fit:695/1*ltqYU7az_yCicWCPmGvwQA.png)

## Challenge Description

> _“There is some interesting information hidden around this site. Can you find it?”_

## Initial Analysis

The target application is a simple static website. As the challenge name “Scavenger Hunt” implies, the flag is not stored in a single location. Instead, it has been fragmented and scattered across common web configuration files, comments, and server artifacts.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*ZRcqdNiKgmHG1zUSZrDM_Q.png)

## Exploitation Flow

## 1\. Part 1: HTML Source Code

Starting with standard client-side reconnaissance, I viewed the page source of the homepage (`Ctrl + U`).

Inside an HTML comment, I found the first segment of the flag:

HTML

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*5mTwHHRXxL22N8CVWiuenw.png)

## 2\. Part 2: CSS Stylesheet (`mycss.css`)

Reviewing the linked assets in the HTML header revealed two external files: `mycss.css` and `myjs.js`.

Inspecting `mycss.css` revealed the second segment left inside a CSS comment:

CSS

_(Checking_ `_myjs.js_` _provided an additional hint pointing towards standard crawler configuration files)._

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*x83ho0xI64Bl4qQt-Z1s4Q.png)

## 3\. Part 3: Web Crawler Directives (`/robots.txt`)

Next, I checked `/robots.txt`. The file contained the third piece of the flag along with a hint to locate the fourth:

Plaintext

```
User-agent: *
Disallow: /index.html

```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*142PEdQ5hUT0roo5Qw5z2w.png)

## 4\. Part 4: Apache Server Configuration (`/.htaccess`)

The hint in `robots.txt` highlighted an **Apache server** and capitalized the word **Access**. In Apache web servers, directory-level configuration is commonly handled by the `.htaccess` file.

Navigating to `/.htaccess` rendered the fourth fragment alongside another clue:

Plaintext

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*mHZm5zqeh1w7wL9KgJF-Cw.png)

## 5\. Part 5: macOS Metadata File (`/.DS_Store`)

The clue in `.htaccess` emphasized building websites on a **Mac** and capitalized the word **Store**.

On macOS, the operating system automatically creates hidden `.DS_Store` (Desktop Services Store) files to hold folder custom attributes and metadata. When developers deploy projects directly from macOS without proper exclusion rules, these files frequently leak into production web roots.

Navigating to `/.DS_Store` exposed the final segment:

Plaintext

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*pJ5yJl70GVc0d3lFyY7dlQ.png)

## 6\. Assembling the Flag

Piecing together all five fragments:

- `picoCTF{th4ts_4_`
- `l0t_0f_`
- `pl4c3s_`
- `2_lO0k`
- `_9588550}`

**Flag:** `picoCTF{th4ts_4_l0t_0f_pl4c3s_2_lO0k_9588550}`

## Vulnerability & Key Takeaways

This challenge highlights **Information Disclosure via Sensitive File Exposure & Metadata Leakage (CWE-200 / CWE-541)**:

- **Strip Client-Side Comments:** Never leave sensitive strings, keys, or breadcrumbs in public HTML/CSS comments.
- **Block Access to Hidden / Dotfiles:** Web servers should be configured to reject direct requests to dotfiles (e.g., `.htaccess`, `.env`, `.git`) by default.
- **Exclude OS Artifacts in Deployments:** Use `.gitignore` and deployment build filters to prevent platform-specific metadata files like `.DS_Store` or `Thumbs.db` from being uploaded to public servers.
