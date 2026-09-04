# CTF Writeups — Muhammad Iqbal Mudzaki

Documenting my CTF solutions, focused on **Web Exploitation & Application Security**.

I'm a web developer transitioning into offensive security. These writeups document my thought process in identifying and exploiting web application vulnerabilities.

## picoCTF

| #   | Challenge        | Category         | Vulnerability                           | Link                                               |
| --- | ---------------- | ---------------- | --------------------------------------- | -------------------------------------------------- |
| 1   | Old Sessions     | Web Exploitation | Sessions Hijack                         | [Writeup](picoctf/old-sessions.md)                 |
| 2   | Crack the Gate 1 | Web Exploitation | Developer Backdoor / Auth Bypass        | [Writeup](picoctf/crack-the-gate-1.md)             |
| 3   | SSTI1            | Web Exploitation | Server-Side Template Injection (Jinja2) | [Writeup](picoctf/ssti1.md)                        |
| 4   | No Sanity 1      | Web Exploitation | Unrestricted File Upload                | [Writeup](picoctf/no-sanity-1.md)                  |
| 5   | Head Dump        | Web Exploitation | Prevent Memory Dump                     | [Writeup](picoctf/head-dump.md)                    |
| 6   | Cookie Monster   | Web Exploitation | Cookie Based                            | [Writeup](picoctf/cookie-monster-secret-recipe.md) |
| 7   | Web Decode       | Web Exploitation | Client Side Exposure                    | [Writeup](picoctf/webdecode.md)                    |
| 8   | Unminify         | Web Exploitation | Information Disclosure                  | [Writeup](picoctf/unminify.md)                     |
| 9   | IntroToBurp      | Web Exploitation | Improper Input Validation               | [Writeup](picoctf/introtoburp.md)                  |
| 10  | Bookmarklet      | Web Exploitation | Client-Side Exposure                    | [Writeup](picoctf/bookmarklet.md)                  |
| 11  | Local Authority  | Web Exploitation | Client-Side Exposure                    | [Writeup](picoctf/local-authority.md)              |
| 12  | Inspect HTML     | Web Exploitation | Information Disclosure                  | [Writeup](picoctf/inspect-html.md)                 |
| 13  | Includes         | Web Exploitation | Information Disclosure                  | [Writeup](picoctf/includes.md)                     |
| 14  | Cookies          | Web Exploitation | Insecure Direct Object Reference (IDOR) | [Writeup](picoctf/cookies.md)                      |
| 15  | Scavenger Hunt   | Web Exploitation | Information Disclosure                  | [Writeup](picoctf/scavenger-hunt.md)               |

## Methodology

Each writeup follows a structured approach:

1. **Reconnaissance** — initial observation, technology identification
2. **Vulnerability Analysis** — why the flaw is exploitable
3. **Exploitation** — step-by-step reproduction (commands/payloads)
4. **Flag** — proof of success
5. **Remediation** — how to prevent this in production

## Tools

`Burp Suite` `nuclei` `subfinder` `httpx` `gobuster` `sqlmap` `wpscan` `gitdumper` `Wappalyzer` `nmap`

## Profile

- Web: [iqbalm.my.id](https://iqbalm.my.id)
- LinkedIn: [linkedin.com/in/iqbalmudzaki](https://linkedin.com/in/iqbalmudzaki)
- GitHub: [pepetgoodboy](https://github.com/pepetgoodboy)
