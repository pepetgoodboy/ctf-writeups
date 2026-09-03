![](https://miro.medium.com/v2/resize:fit:697/1*1rQ65wRzPSVvNnlA4oyygQ.png)

## Challenge Description

> _“Who doesn’t love cookies? Try to figure out the best one.”_

## Initial Analysis

The challenge name and description point directly toward **HTTP Cookie manipulation**. The prompt invites us to _“figure out the best one”_, indicating that different cookie states or values control what the application displays.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*UBT1URv8HttJyAVV7MthlA.png)

## Exploitation Flow

## 1\. Initial Reconnaissance

As part of standard recon, I first queried `/robots.txt` to inspect potential hidden routes, but the server responded with a **404 Not Found**.

Returning to the home page, I tested the input form with an arbitrary string (`test`). The application rejected it with an error message:

> “That doesn’t appear to be a valid cookie.”

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*nzJJCuLW2N1hOUMGYt0cGA.png)

## 2\. Observing Valid Input Behavior

I then submitted the suggested placeholder value: `snickerdoodle`.

This time, the form redirected successfully to `/check`, rendering a message confirming the cookie type.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*yNqKsljS-O_aArIRNXrlMw.png)

## 3\. Inspecting the Cookie Value

Opening Developer Tools (`F12`) and navigating to **Application > Cookies**, I spotted a cookie named `name` with a numerical value of `0`.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*GrLPpVEe_8-k4xO1hulNMw.png)

This revealed how the backend handles state: rather than validating user session tokens, it uses a simple integer index stored in the `name` cookie to reference specific items.

## 4\. Enumerating Cookie Values & Finding the Flag

Following the challenge hint to find the “best” cookie, I manually iterated through numerical values for the `name` cookie (or by automating a simple loop in Burp Intruder/cURL from 1 to 20) and refreshed the page.

- `name=0` -> snickerdoodle
- `name=1` -> chocolate chip
- `...`

When the value reached `18`, the server rendered a special response containing the flag:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*vYruQe9OXqoZWmufl3vZbg.png)

**Flag:** `picoCTF{3v3ry1_l0v3s_c00k135_a4dadb49}`

## Vulnerability & Remediation

This challenge demonstrates **Insecure Direct Object Reference (IDOR) / Improper Session State Handling (CWE-639 / CWE-565)**.

The application relied on a predictable, client-controlled cookie value without enforcing backend authorization checks:

- **Never Trust Client-Side State:** Critical logic or authorization states should never rely on plain, tamperable cookie parameters.
- **Use Cryptographically Signed Sessions:** Use secure, tamper-proof session identifiers (such as server-managed session IDs or signed JWTs) rather than sequential integers exposed in plain text.
