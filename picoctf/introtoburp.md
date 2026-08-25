![](https://miro.medium.com/v2/resize:fit:689/1*J9fju3OBCt3E_2RU-vnJYA.png)

## Challenge Description

> _“No Description”_

## Hints:

- _“Try using burpsuite to intercept request to capture the flag.”_
- _“Try mangling the request, maybe their server-side code doesn’t handle malformed requests very well.”_

## Initial Analysis

When opening the challenge instance, the root path (`/`) presents a simple user registration form. The challenge hints suggest using **Burp Suite** to intercept HTTP traffic and "mangle" (tamper with) requests to exploit improper input handling or state validation on the server side.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*ofB7e8qlC6Q9MB1nSnT2qQ.png)

## Exploitation Flow

## 1\. Intercepting Traffic with Burp Suite

I launched Burp Suite and routed the browser traffic through the Burp Proxy to inspect and intercept HTTP requests.

First, I submitted arbitrary registration details through the form at `/`. Upon successful registration, the application redirected to an OTP verification page at `/dashboard`.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*R6BT8rFB0pN_ePtH9bIjoQ.png)

## 2\. Testing Normal OTP Submission

Attempting to enter a dummy OTP (e.g., `1234`) resulted in an **"Invalid OTP"** error message on the page.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*rD_ZoHLVMxu3dkPyFI3Sqw.png)

## 3\. Mangling the Request Body

Taking cues from the hint about server-side code failing to handle malformed requests, I turned on **Interception** in Burp Suite and submitted another OTP verification request.

The captured `POST` request included the parameter in the body:

HTTP

```
POST /dashboard HTTP/1.1
Host: <TARGET_HOST>
Content-Type: application/x-www-form-urlencoded
Content-Length: 8
```

```
otp=1234
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*1eJYqLGKpue4B3pnvmuV0g.png)

To test how the backend handles missing parameters, I removed the `otp=1234` parameter entirely, leaving the request body empty:

HTTP

```
POST /dashboard HTTP/1.1
Host: <TARGET_HOST>
Content-Type: application/x-www-form-urlencoded
Content-Length: 0
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*uess_jG-ncgma4UD_MfUUg.png)

## 4\. Retrieving the Flag

I forwarded the modified request to the server. Due to inadequate error handling on undefined/null values in the backend validation logic, the OTP verification check was completely bypassed.

The server responded with the dashboard view revealing the flag:

![](https://miro.medium.com/v2/resize:fit:532/1*61GUvpFZNJJkqfC0t2v-hQ.png)

**Flag:** `picoCTF{#0TP_Bypvss_SuCc3$S_9090d63c}`

## Vulnerability & Remediation

This issue stems from **Improper Input Validation & Flawed Business Logic** (CWE-20 / CWE-840).

When the backend logic verifies the OTP parameter, it likely checks if the supplied value matches a stored value without properly asserting that the parameter **exists and is non-empty** first. When the parameter is omitted, the condition (such as checking against `undefined` or a falsy comparison) unintentionally evaluates as valid.

## How to Fix:

- **Strict Existence Checks:** Ensure all critical authentication parameters are explicitly verified for existence and valid data types before running comparison logic.
- **Fail Closed:** Default authentication flows should always fail closed unless all validation checks pass explicitly.
