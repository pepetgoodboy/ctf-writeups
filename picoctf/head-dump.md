![](https://miro.medium.com/v2/resize:fit:700/1*IhsVggd1GJ-zJqcFXihZBQ.png)

## Challenge Description

> _“The application is a simple blog website where you can read articles about various topics, including an article about API Documentation. Your goal is to explore the application and find the endpoint that generates files holding the server’s memory, where a secret flag is hidden.”_

## Initial Analysis

The target application presents a simple blog website featuring various articles, including one related to API documentation. The description specifically hints at finding an exposed endpoint that generates a dump of the server’s memory.

Using the **Wappalyzer** extension and analyzing the application’s behavior, I identified the backend tech stack:

- **Node.js** & **Express**
- **Swagger** (for API documentation)

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*BrzZSGRvY2jyGGTE4buO6g.png)

## Exploitation Flow

## 1\. Discovering API Documentation

Given the hint about API documentation, I attempted to navigate to standard API documentation routes. Accessing `/api-docs` successfully routed me to the **Swagger UI** page, which exposed the available API endpoints.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*KXx-hJuRYaFSupIu-krqQA.png)

## 2\. Identifying the Target Endpoint

While reviewing the Swagger UI documentation, I found a diagnostic endpoint:

Plaintext

```
GET /heapdump
```

The endpoint description explicitly mentioned **“Diagnosing the memory allocation”**, matching the hint about retrieving the server’s memory contents.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*gyO0dW-7hdFCCJwIku9lTQ.png)

## 3\. Downloading the Heap Snapshot

Accessing the `/heapdump` endpoint triggered an automatic download of a `.heapsnapshot` file containing the memory state of the running Node.js process.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*QpfqDQwP8PVyYjEQ66z13w.png)

## 4\. Extracting the Flag

Heap dumps store strings and objects allocated in server memory, which often include sensitive configuration data or secrets processed by the application.

Rather than loading the large file into heap profiling tools (like Chrome DevTools), I used `grep` directly in the terminal to search for the standard picoCTF flag format:

Bash

```
grep -o "picoCTF{.*}" <filename>.heapsnapshot
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*UiD3BlCaZdKqE_IrOiaNQA.png)

The flag string was located directly within the memory dump file and displayed in the terminal.

## Conclusion & Remediation

Exposing diagnostic endpoints like Node.js heap dumps (`v8.getHeapSnapshot()` or packages like `heapdump`) in a production environment introduces severe data leakage risks. Heap memory often retains sensitive data in plain text, including API keys, user tokens, and environment variables.

To prevent memory dump exposure:

- **Restrict Diagnostic Endpoints:** Ensure diagnostic routes like `/heapdump` are disabled in production or strictly restricted to administrative users via authentication middleware.
- **Remove Debug Tools in Production:** Strip profiling and debugging libraries prior to deploying applications to public environments.
