# 🔬 Lab-6: DOM XSS in jQuery Selector Sink Using a `hashchange` Event

## 📌 Overview

This lab demonstrates a **DOM-based Cross-Site Scripting (XSS)** vulnerability on the **home page** of the application 🧨. The issue arises because the application uses **jQuery’s `$()` selector function** to automatically scroll to a specific post, where the post title is taken directly from the **`location.hash`** property.

Because the value from `location.hash` is not properly sanitized, an attacker can inject malicious HTML or JavaScript, resulting in arbitrary code execution in the victim’s browser.

---

## 🎯 Lab Objective

To successfully solve this lab, you must:

* Exploit the DOM-based XSS vulnerability
* Deliver a working exploit to the victim
* Trigger execution of the `print()` function in the victim’s browser 🖨️

---

## 🧠 Vulnerability Explanation

### 🔑 Key Points

* User input is taken directly from `location.hash`
* jQuery’s `$()` selector is used as the sink
* The selector dynamically processes attacker-controlled input
* No input validation or sanitization is applied ❌

### ⚠️ Why This Is Dangerous

jQuery selectors interpret HTML when provided with attacker-controlled input. By injecting malicious markup into the URL fragment (`#`), an attacker can execute JavaScript whenever the page reacts to the `hashchange` event 💥.

---

## 🪜 Step-by-Step Walkthrough

### 📝 Step 1: Open the Lab Website

Access the lab and carefully read the **overview section**. This explains that the page uses the URL fragment (`location.hash`) to auto-scroll to posts.

---

### 🔍 Step 2: Identify the Injection Point

The vulnerable input is passed after the `#` symbol in the URL, for example:

```
#SomePostTitle
```

This value is processed by jQuery without sanitization.

---

### 🧪 Step 3: Test for XSS Using an Alert Payload

To confirm the vulnerability, inject a simple test payload using the hash fragment:

```
#<img src=x onerror=alert(2)>
```

✅ If an alert box appears, the DOM XSS vulnerability is confirmed.

---

### 🛠️ Step 4: Prepare the Final Exploit Payload

The goal of this lab is to call the `print()` function in the victim’s browser. To achieve this, an exploit is delivered using the **Exploit Server** and an `<iframe>` element.

Use the following payload:

```
<iframe src="https://0a5600a103e7e7ed80dd1234003d0041.web-security-academy.net/#" 
        onload="this.src+='%3Cimg%20src=x%20onerror=print()%3E'"></iframe>
```

### 🔎 Payload Explanation

* The iframe initially loads the vulnerable page
* The `onload` event appends a malicious `<img>` tag to the URL fragment
* The injected image triggers `onerror`, which calls `print()`

---

### 🚀 Step 5: Test the Exploit

Click **View exploit** on the Exploit Server.

🖨️ If the browser print dialog appears, the exploit is working correctly.

---

### 📬 Step 6: Deliver the Exploit to the Victim

Once confirmed:

1. Click **Deliver exploit to victim**
2. The victim’s browser executes the payload
3. The `print()` function is triggered

✅ **The lab is successfully solved!**

---

## 🧩 Root Cause Analysis

* **Source:** `location.hash`
* **Sink:** jQuery `$()` selector
* **Trigger:** `hashchange` event
* **Missing Controls:** Input validation and output encoding

---

## 🛡️ Security Takeaways

* Never trust data from `location.hash`
* Avoid passing user input directly into jQuery selectors
* Sanitize and encode all DOM-based inputs
* Treat URL fragments as untrusted input 🚫

---

## 🏁 Conclusion

This lab highlights how client-side logic combined with unsafe jQuery selectors can introduce **critical DOM-based XSS vulnerabilities**. Because the attack occurs entirely in the browser, it often bypasses server-side defenses.

Understanding how **hash-based navigation**, **jQuery selectors**, and **DOM events** interact is essential for both exploiting and securing modern web applications 🔐.
