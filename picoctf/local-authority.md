![](https://miro.medium.com/v2/resize:fit:687/1*iORrhFopPv-l8ZhTdG026g.png)

## Challenge Description

> _“Can you get the flag?”_

## Hints:

- _“How is the password checked on this website?”_

## Initial Analysis

When navigating to the target website, we are presented with a simple authentication form asking for a username and password. The challenge hint (_“How is the password checked on this website?”_) suggests investigating the authentication flow to see if the verification occurs server-side or improperly in the client browser.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*mue_nHQCnx5J6hFJr1gwqw.png)

## Exploitation Flow

## 1\. Testing Default Login Behavior

I started by submitting dummy credentials (`username: test`, `password: test`) to observe how the application handles failed authentications.

The application submitted a POST request to `login.php`, resulting in a **"Log In Failed"** status page.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*kLOxUu4TfGpVQc9KqtXftQ.png)

## 2\. Inspecting the Page Source

To inspect the scripts handling the failed login response, I opened the page source view (`Ctrl + U`) on `login.php`.

While reviewing the HTML headers and embedded assets, I noticed an external JavaScript file “secure.js”

![](https://miro.medium.com/v2/resize:fit:698/1*KWhkOgWdYftAvSP2PaeOTg.png)

## 3\. Reviewing `secure.js`

Navigating directly to `/secure.js` revealed the authentication logic. Instead of validating credentials against a secure backend database, the application executed a client-side JavaScript function called `checkPassword()` containing hardcoded credentials:

```
function checkPassword(username, password) {
  if( username === 'admin' && password === 'strongPassword098765' ) {
    ...
  }
}
```

![](https://miro.medium.com/v2/resize:fit:698/1*c7_1GaleKDeKXCgX1-ig0Q.png)

## 4\. Authenticating & Retrieving the Flag

With the valid credentials exposed in plaintext:

- **Username:** `admin`
- **Password:** `strongPassword098765` _(or the exact matching password in the file)_

I returned to the main login form, entered the credentials, and submitted the request. The application accepted the authentication and printed the flag.

![](https://miro.medium.com/v2/resize:fit:698/1*DP19xFG7UOVImgQEGL-I1A.png)

**Flag:** `picoCTF{j5_15_7r4n5p4r3n7_a8788e61}`

## Vulnerability & Remediation

This challenge demonstrates **Client-Side Authentication / Hardcoded Credentials** (CWE-603 / CWE-798).

Authentication checks must **never** be performed in client-side code:

- **Server-Side Validation:** Always process and verify user credentials strictly on the backend server using secure password hashing algorithms (e.g., bcrypt, Argon2).
- **Never Expose Credentials in Static Assets:** Frontend JavaScript files are publicly accessible to anyone visiting the site. Hardcoded secrets in client-side scripts are inherently compromised.
