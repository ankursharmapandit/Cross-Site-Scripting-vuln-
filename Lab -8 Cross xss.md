# 🔬 Lab-8 Stored XSS into Anchor `href` Attribute with Double Quotes HTML-Encoded

## 📌 Overview

This lab demonstrates a **stored Cross-Site Scripting (XSS)** vulnerability in the **comment functionality** of the application 🧨. Although double quotes (`"`) are HTML-encoded, the application still allows dangerous input to be stored and later executed because user-controlled data is inserted into an **anchor (`<a>`) tag’s `href` attribute** without proper validation.

As a result, an attacker can store a malicious payload that executes JavaScript when another user clicks on the **comment author name**.

---

## 🎯 Lab Objective

To successfully solve this lab, you must:

* Exploit the stored XSS vulnerability in the comment feature
* Inject a malicious payload into the comment author field
* Trigger execution of the `alert()` function when the author name is clicked ⚠️

---

## 🧠 Vulnerability Explanation

### 🔑 Key Observations

* User input is stored persistently in the application database
* Double quotes are HTML-encoded, limiting some injection techniques
* The application still allows **JavaScript URLs** inside the `href` attribute
* The stored payload executes when a victim interacts with the page ❌

### ⚠️ Why This Is Dangerous

Stored XSS is especially severe because the payload executes for **every user** who views the affected content. Even with partial encoding in place, failing to validate URL schemes allows attackers to execute arbitrary JavaScript using `javascript:` URLs 💥.

---

## 🪜 Step-by-Step Walkthrough

### 📝 Step 1: Review the Lab Overview

Begin by reading the lab description to understand that the vulnerability exists in the **comment functionality** and affects the comment author link.

---

### 🌐 Step 2: Visit the Blog Post

Navigate to any blog post that allows users to submit comments.

You will see a form that typically includes:

* Comment text
* Author name
* Website (URL) field

---

### 🧪 Step 3: Identify the Injection Point

Submit a normal comment and observe how the **author name** is rendered as a clickable link. The application uses this value inside an anchor tag’s `href` attribute.

This confirms the vulnerable context.

---

### 💉 Step 4: Craft the Stored XSS Payload

In the **Website / URL** or relevant input field that controls the link destination, submit the following payload:

```
javascript:alert(1)
```

Even though double quotes are HTML-encoded, the application does not restrict the `javascript:` scheme.

---

### 🚀 Step 5: Trigger the Payload

1. Submit the comment
2. Return to the blog post
3. Click on the **comment author name**

⚡ An alert box appears, confirming successful exploitation.

✅ **The lab is successfully solved!**

---

## 🧩 Root Cause Analysis

* **Source:** Stored comment input
* **Sink:** Anchor tag `href` attribute
* **Context:** URL-based JavaScript execution
* **Missing Controls:** URL scheme validation and output sanitization

---

## 🛡️ Security Takeaways

* Encoding quotes alone is **not sufficient** to prevent XSS
* Never allow `javascript:` URLs in user-controlled links 🚫
* Always validate and sanitize stored input
* Use allowlists for safe URL schemes such as `http` and `https` 🔐

---

## 🏁 Conclusion

This lab highlights how **stored XSS vulnerabilities can persist even when partial HTML encoding is applied**. Failing to validate URL schemes in anchor tags allows attackers to store malicious payloads that execute on user interaction.

Understanding **stored XSS**, **URL-based attacks**, and **HTML attribute contexts** is critical for building secure web applications 🚨.
