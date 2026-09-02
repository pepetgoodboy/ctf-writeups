![](https://miro.medium.com/v2/resize:fit:698/1*4XFQ1Mvoz-pszvLhn09MNw.png)

## Challenge Description

> _“Can you get the flag?”_

## Hints:

- _“Is there more code than what the inspector initially shows?”_

## Initial Analysis

Upon opening the target instance, the page renders a simple layout with basic static content. The hint questions whether there is additional code beyond what the initial HTML inspector reveals, hinting at external resources linked to the page.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*nGweWVowJQm61n0CFBDXBw.png)

## Exploitation Flow

## 1\. Initial Reconnaissance

As standard practice, I first checked common endpoints like `/robots.txt` to see if any hidden paths or directories were restricted, but it returned a **404 Not Found**.

## 2\. Inspecting Linked Assets

Next, I viewed the raw page source (`Ctrl + U`) to look for external asset references. Inside the `<head>` and `<body>` tags, two external static resources were linked:

- A CSS stylesheet: `style.css`
- A JavaScript file: `script.js`

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*YtmpWoiae8v1N-9IT81qLw.png)

## 3\. Extracting the Split Flag Parts

### Part 1: `style.css`

Navigating directly to the stylesheet route (`/style.css`), I inspected the file contents. Inside a CSS comment block, the author left the first half of the flag:

CSS

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*Dj3spdr-29z6AUnAeTpIbA.png)

### Part 2: `script.js`

Next, I checked the JavaScript file (`/script.js`). The second half of the flag was similarly hidden inside a JavaScript comment:

JavaScript

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*iLGQD1VBqCgGrrc4sy-lmQ.png)

## 4\. Assembling the Flag

Concatenating both segments together reconstructed the complete flag string:

`picoCTF{1nclu51v17y_1of2_f7w_2of2_b8f4b022}`

## Vulnerability & Remediation

This challenge falls under **Information Exposure Through Comments / Static Assets (CWE-615 / CWE-200)**.

Splitting sensitive information across different static files (CSS, JS) provides zero protection against enumeration:

- **Public Asset Exposure:** Any file linked in the DOM is downloaded and cached by the client, making all included assets accessible via direct URL navigation.
- **Build Pipeline Stripping:** Configure modern build tools (such as Vite, Webpack, or Terser) to automatically strip all code comments and metadata from CSS and JS bundles during production builds.
