# Lab-13 Stored DOM XSS — Blog Comment Functionality

## Overview

This lab demonstrates a **Stored DOM-based Cross-Site Scripting (XSS)** vulnerability in a blog’s comment functionality.  
The objective is to exploit this vulnerability and successfully trigger the JavaScript `alert()` function.

Although user input is partially encoded by the application, flawed client-side logic allows an attacker to bypass these protections and execute arbitrary JavaScript.

---

## Vulnerability Summary

- The application stores user comments on the server.
- Comments are later rendered in the browser using **DOM manipulation**.
- The application attempts to sanitize input by encoding angle brackets (`<` and `>`).
- However, it only encodes the **first occurrence** of these characters using a JavaScript `replace()` function.
- This results in a **Stored DOM XSS vulnerability**.

---

## Root Cause Analysis

The vulnerability exists due to **improper input sanitization in client-side JavaScript**:

- The `replace()` function is used **without a global flag**, meaning only the **first match** is replaced.
- Any subsequent malicious HTML or JavaScript remains unfiltered.
- When the stored comment is rendered in the DOM, the browser interprets the remaining payload as executable code.

---

## Exploitation Strategy

To exploit this vulnerability:
1. Inject multiple HTML tags.
2. Allow the application to encode only the first occurrence.
3. Use a second malicious tag that remains unencoded and executes JavaScript.

---

## Payload Used

```html
<><img src=x onerror=alert(1)>
Why This Works
The first < and > are encoded by the application.

The second <img> tag remains untouched.

The browser parses the <img> tag.

The invalid image source (src=x) triggers the onerror event.

The alert(1) function executes successfully.

Step-by-Step Exploitation
Open the lab website.

Navigate to any blog post.

Scroll down to the comment section.

Enter the following payload in the comment input field:

<><img src=x onerror=alert(1)>
Submit the comment.

Reload the page or revisit the blog post.

Observe the JavaScript alert() pop up — lab solved 🎉

Impact
An attacker could leverage this vulnerability to:

Execute arbitrary JavaScript in victims’ browsers

Steal session cookies

Perform actions on behalf of logged-in users

Deface website content

Because the payload is stored, it affects all users who view the page.

Mitigation Recommendations
To prevent this type of vulnerability:

Perform server-side input validation.

Avoid using innerHTML or unsafe DOM sinks.

Use context-aware output encoding.

Apply global replacements or proper sanitization libraries.

Implement a strict Content Security Policy (CSP).
