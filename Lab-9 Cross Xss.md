# 🔬 Lab-9 Reflected XSS into a JavaScript String with Angle Brackets HTML-Encoded

## 📌 Overview

This lab demonstrates a **reflected Cross-Site Scripting (XSS)** vulnerability in the **search query tracking functionality** of the application 🧨. In this case, angle brackets (`<` and `>`) are HTML-encoded, which prevents direct HTML injection. However, the application remains vulnerable because the user input is reflected **inside a JavaScript string** without proper escaping.

By breaking out of the JavaScript string context, an attacker can inject and execute arbitrary JavaScript in the victim’s browser.

---

## 🎯 Lab Objective

To successfully solve this lab, you must:

* Identify the reflected XSS vulnerability in the search feature
* Break out of the JavaScript string context
* Execute the `alert()` function ⚠️

---

## 🧠 Vulnerability Explanation

### 🔑 Key Observations

* Angle brackets are HTML-encoded, blocking classic HTML-based payloads ❌
* User input is reflected directly inside a JavaScript string
* Quotes are not properly escaped
* JavaScript context injection is possible

### ⚠️ Why This Is Dangerous

Even when HTML encoding is applied, **JavaScript string injection** can still lead to XSS if user input is embedded inside a script block without proper escaping. By closing the string and injecting new JavaScript code, an attacker gains execution control 💥.

---

## 🪜 Step-by-Step Walkthrough

### 📝 Step 1: Open the Search Functionality

Navigate to the website and locate the **search feature**.

---

### 🔍 Step 2: Test Input Reflection

Search for a simple value such as:

```
hello
```

Inspect the page source or developer tools and observe that the search term is reflected inside JavaScript code, similar to:

```
var searchTerm = 'hello';
```

This confirms that user input is placed directly inside a JavaScript string.

---

### 🧪 Step 3: Craft the XSS Payload

To break out of the JavaScript string and inject code, use the following payload:

```
a';alert(1)//
```

### 🔎 Payload Breakdown

* `'` closes the existing JavaScript string
* `;` terminates the original statement
* `alert(1)` executes JavaScript
* `//` comments out the remaining code

---

### 🚀 Step 4: Execute the Payload

1. Enter the payload into the search box
2. Submit the search request
3. The injected JavaScript executes immediately

⚡ An alert box appears, confirming successful exploitation.

✅ **The lab is successfully solved!**

---

## 🧩 Root Cause Analysis

* **Source:** Search query parameter
* **Sink:** JavaScript string context
* **Context:** Inline JavaScript execution
* **Missing Controls:** Proper JavaScript string escaping

---

## 🛡️ Security Takeaways

* HTML encoding alone does **not** protect JavaScript contexts
* Always escape user input based on its execution context
* Use safe APIs for embedding data into JavaScript
* Apply Content Security Policy (CSP) where possible 🔐

---

## 🏁 Conclusion

This lab highlights how **reflected XSS can occur even when angle brackets are HTML-encoded**. When user input is embedded inside JavaScript code, failing to properly escape quotes can allow attackers to break out of string literals and execute arbitrary scripts.

Understanding **JavaScript execution contexts**, **string boundaries**, and **output encoding** is essential for identifying and preventing reflected XSS vulnerabilities 🚨.
