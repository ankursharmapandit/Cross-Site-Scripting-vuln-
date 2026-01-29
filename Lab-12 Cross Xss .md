# Lab-12 Walkthrough: Reflected DOM XSS

## Overview

This lab demonstrates a **Reflected DOM-based Cross-Site Scripting (XSS)** vulnerability.  
A reflected DOM XSS occurs when:

1. The **server-side application** processes input from a request.
2. That input is **reflected back in the HTTP response**.
3. A **client-side JavaScript function** processes the reflected data in an unsafe way.
4. The data is written to a **dangerous sink**, such as `eval()`, resulting in JavaScript execution.

The goal of this lab is simple:

> **Create an injection that triggers the `alert()` function.**

---

## Understanding the Vulnerability

This application contains a **search functionality** that reflects user input back to the page. The reflected value is later processed by JavaScript using the `eval()` function, which introduces the vulnerability.

### Why `eval()` Is Dangerous

The `eval()` function executes any string passed to it as JavaScript code.  
If user-controlled input reaches `eval()` without proper sanitization, an attacker can inject and execute arbitrary JavaScript.

---

## Step 1: Identify the Reflection

1. Open the lab website.
2. Use the search functionality and search for:
hello

3. Right-click on the page and select **View Page Source**.
4. You will see that the value `hello` appears **exactly once** in the source code.

This confirms that:
- User input is reflected in the response.
- The reflection is likely being used by JavaScript.

---

## Step 2: Analyze the JavaScript Code

In the page source, locate the following JavaScript snippet:

```javascript
eval('var searchResultsObj = ' + this.responseText);
What’s Happening Here?
this.responseText contains the server’s response (which includes your search input).

The response is concatenated into a string.

eval() executes the resulting string as JavaScript.

This means your input is being inserted directly into a JavaScript context without validation.

Step 3: Understand the Exploitation Logic
The server response is expected to be a JSON object.

To exploit this:

We close the JSON structure.

Escape out of the string context.

Inject our own JavaScript payload.

Comment out the remaining code to prevent syntax errors.

Step 4: Craft the Payload
Use the following payload as the search input:

\"};alert(1);// 
Payload Breakdown

Part	Purpose
\"	Escapes the existing string
}	Closes the JSON object
;	Ends the JavaScript statement
alert(1)	Executes our JavaScript payload
//	Comments out the remaining code

Step 5: Trigger the XSS
Enter the payload into the search box.

Submit the search request.

The injected JavaScript executes.

The browser displays an alert box, confirming successful exploitation.

✅ Lab Solved
