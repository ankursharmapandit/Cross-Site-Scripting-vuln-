# 🔐 Cross-Site-Scripting-vuln

## ❓ What is Cross-Site Scripting (XSS)?

**Cross-Site Scripting (XSS)** is a web vulnerability that allows an attacker to inject **malicious JavaScript** into a trusted website.

When another user visits the affected site, the injected script runs in their browser as if it were legitimate code from the website. This happens because the browser trusts the website’s content.

---

## ⚠️ Why XSS Is Dangerous

XSS vulnerabilities are dangerous because they allow attackers to:

- 🍪 **Steal session cookies**  
  (Data files stored in the browser that maintain user login sessions)

- 🔑 **Steal authentication tokens**  
  (Secret digital keys used to validate a user’s identity)

- 🕵️ **Impersonate users**  
  Perform actions as the victim

- 🚨 **Redirect users to malicious websites**  
  Often used in phishing or malware attacks

---

## 🧠 Types of XSS

There are multiple types of Cross-Site Scripting vulnerabilities:

1. 🗄️ **Stored XSS**
2. 🔁 **Reflected XSS**
3. 🧩 **DOM-Based XSS**

---

## 🧪 Lab 1: Reflected XSS into HTML Context (Nothing Encoded)

### 📝 Lab Description

This lab contains a **simple reflected Cross-Site Scripting (XSS) vulnerability** in the **search functionality**.

All user-supplied input is reflected directly into the HTML response **without any encoding or sanitization**, making the application vulnerable to XSS attacks.

---

### 🔍 Vulnerability Explanation

- 🔎 User input is taken through a **search box**
- 📄 The input is reflected directly on the response page
- ❌ No HTML encoding or sanitization is applied
- 💻 HTML and JavaScript input is executed by the browser

---

### 📌 Example

**User Input:**

**Server Response:**

Since the input is not escaped or encoded, it is treated as **trusted HTML**.

---

### 💥 XSS Payload Execution

Enter the following payload into the search box:

```html
<script>alert(1)</script>

✅ Result:

The payload is reflected without encoding

The browser executes the JavaScript

An alert box appears

This confirms a Reflected XSS vulnerability.

🧩 Root Cause

❌ User input is trusted without validation

❌ No output encoding

❌ Unsafe handling of reflected data
