# 🔬 Lab-11 DOM XSS in AngularJS Expression with Angle Brackets and Double Quotes HTML-Encoded

## 📌 Overview

This lab demonstrates a **DOM-based Cross-Site Scripting (XSS)** vulnerability in an **AngularJS expression** within the **search functionality** of the application 🧨. Although angle brackets (`<`, `>`) and double quotes (`"`) are HTML-encoded, the application remains vulnerable due to unsafe handling of **AngularJS template expressions**.

AngularJS automatically evaluates expressions enclosed in **double curly braces (`{{ }}`)** when the page contains an `ng-app` directive. This allows attackers to execute JavaScript expressions even when traditional HTML injection is blocked.

---

## 🎯 Lab Objective

To successfully solve this lab, you must:

* Exploit the DOM-based XSS vulnerability in the AngularJS search feature
* Inject a malicious AngularJS expression
* Execute the `alert()` function ⚠️

---

## 🧠 Vulnerability Explanation

### 🔑 Key Concepts

* AngularJS scans HTML nodes that include the `ng-app` directive
* Expressions inside `{{ }}` are automatically evaluated
* Angle brackets and quotes being encoded does **not** stop AngularJS expression execution
* User input is directly embedded into an AngularJS expression context ❌

### ⚠️ Why This Is Dangerous

AngularJS expressions are powerful and can access JavaScript constructors and functions. If user input is reflected into an AngularJS template without sanitization, attackers can execute arbitrary JavaScript—even without injecting HTML tags 💥.

---

## 🪜 Step-by-Step Walkthrough

### 📝 Step 1: Open the Search Functionality

Navigate to the vulnerable page and locate the **search box**.

---

### 🔍 Step 2: Confirm Reflection

Search for a simple value such as:

```
hello
```

Observe that the input is reflected on the page and processed by AngularJS, confirming that the search result is evaluated within an AngularJS context.

---

### 🧪 Step 3: Identify the Injection Context

Since angle brackets and quotes are encoded, traditional HTML-based XSS payloads will not work. Instead, AngularJS expressions inside `{{ }}` can still be executed.

---

### 💉 Step 4: Craft the AngularJS XSS Payload

Use the following AngularJS expression payload:

```
{{$on.constructor('alert(1)')()}}
```

### 🔎 Payload Breakdown

* `$on` → An existing AngularJS object
* `constructor` → Accesses the Function constructor
* `'alert(1)'` → JavaScript code to execute
* `()` → Immediately invokes the constructed function

This technique is commonly referenced in **AngularJS XSS cheat sheets** and works even when HTML characters are encoded.

---

### 🚀 Step 5: Execute the Payload

1. Paste the payload into the search field
2. Submit the search
3. AngularJS evaluates the expression

⚡ An alert box appears, confirming successful exploitation.

✅ **The lab is successfully solved!**

---

## 🧩 Root Cause Analysis

* **Source:** Search input parameter
* **Sink:** AngularJS expression evaluation
* **Context:** Template expression execution
* **Missing Controls:** AngularJS expression sanitization

---

## 🛡️ Security Takeaways

* Encoding HTML characters does **not** prevent AngularJS expression injection
* Never reflect user input directly into AngularJS templates
* Use `$sanitize` or disable expression evaluation where possible
* Avoid using AngularJS on untrusted content 🔐

---

## 🏁 Conclusion

This lab highlights how **AngularJS expression injection** can lead to DOM-based XSS even when traditional HTML injection vectors are blocked. The powerful expression engine in AngularJS makes improper input handling extremely dangerous.

Understanding **AngularJS internals**, **expression contexts**, and **DOM-based sinks** is critical for identifying and preventing this class of XSS vulnerabilities 🚨.
