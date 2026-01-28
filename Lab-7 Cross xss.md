# 🔬 Lab-7 Reflected XSS into Attribute with Angle Brackets HTML-Encoded

## 📌 Overview

This lab demonstrates a **reflected Cross-Site Scripting (XSS)** vulnerability in the **search blog functionality** of the application 🧨. In this scenario, angle brackets (`<` and `>`) are HTML-encoded, which prevents direct HTML injection. However, the application remains vulnerable because **user input is reflected into an HTML attribute without proper encoding**.

By exploiting this weakness, an attacker can inject a malicious attribute and successfully execute JavaScript in the victim’s browser.

---

## 🎯 Lab Objective

To solve this lab, you must:

* Identify the reflected XSS vulnerability in the search feature
* Inject a malicious attribute into an HTML element
* Trigger execution of the `alert()` function ⚠️

---

## 🧠 Vulnerability Explanation

### 🔑 Key Observations

* Angle brackets are HTML-encoded, blocking classic `<script>` payloads ❌
* User input is reflected into an existing HTML attribute
* One reflection point is safely encoded, while another is **not encoded**
* Attribute injection is possible by breaking out of the quoted context

### ⚠️ Why This Is Dangerous

Even when HTML tags are encoded, **attribute-based XSS** can still occur if user input is placed inside an attribute value without proper escaping. By injecting a new attribute and an event handler, JavaScript execution becomes possible 💥.

---

## 🪜 Step-by-Step Walkthrough

### 📝 Step 1: Open the Search Blog Functionality

Navigate to the blog page and locate the **search input field** labeled:

```
Search the blog...
```

---

### 🔍 Step 2: Perform a Normal Search

Search for a benign value such as:

```
hello
```

Inspect the page source and observe how your input is reflected. You will notice **two reflection points**:

* One location where the input is HTML-encoded ✅
* A second location inside an HTML attribute that is **not properly encoded** ❌

---

### 🧪 Step 3: Identify the Vulnerable HTML Context

The vulnerable reflection appears inside an input element similar to:

```
<input type="text" placeholder="Search the blog..." name="search" value="hello">
```

Since the value attribute is not safely encoded, it can be exploited using attribute injection.

---

### 💉 Step 4: Craft the XSS Payload

To break out of the `value` attribute and inject a new event handler, use the following payload:

```
a" onfocus="alert(1)" autofocus
```

---

### 🚀 Step 5: Execute the Payload

1. Paste the payload into the search box
2. Submit the search request
3. The injected `autofocus` attribute automatically focuses the input field
4. The `onfocus` event triggers `alert(1)` ⚡

✅ The alert box confirms successful exploitation.

---

## 🧩 Root Cause Analysis

* **Source:** Search query parameter
* **Sink:** HTML attribute (`value`)
* **Context:** Attribute injection
* **Missing Controls:** Proper attribute encoding and output escaping

---

## 🛡️ Security Takeaways

* Encoding angle brackets alone is **not sufficient**
* Always contextually encode output based on where user input is placed
* Escape quotes when reflecting input inside attributes
* Use secure templating frameworks where possible 🔐

---

## 🏁 Conclusion

This lab highlights how **reflected XSS can still be exploited even when basic HTML encoding is in place**. Attribute injection remains a common and dangerous attack vector when applications fail to properly escape user input.

Understanding **HTML contexts**, **attribute boundaries**, and **event handlers** is essential for identifying and preventing reflected XSS vulnerabilities 🚨.
