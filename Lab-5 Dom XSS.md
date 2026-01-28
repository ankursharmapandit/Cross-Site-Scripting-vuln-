# 🔬 Lab-5: DOM XSS in jQuery Anchor `href` Attribute Sink Using `location.search`

## 📌 Overview

This lab demonstrates a **DOM-based Cross-Site Scripting (XSS)** vulnerability 🧨 in the *Submit Feedback* page. The issue occurs because the application uses **jQuery’s `$()` selector** to locate an anchor (`<a>`) element and dynamically updates its `href` attribute using **unsanitized input from `location.search`**.

As a result, an attacker can manipulate URL parameters to inject a **malicious JavaScript payload**, leading to arbitrary script execution in the victim’s browser 😈.

---

## 🎯 Lab Objective

To successfully solve this lab, you must:

* Exploit the DOM-based XSS vulnerability
* Modify the **“Back”** link so that it executes the following payload:

```
javascript:alert(document.cookie)
```

---

## 🧠 Vulnerability Explanation

### 🔑 Key Points

* The application reads user-controlled input from `location.search`
* This input is directly assigned to an anchor tag’s `href` attribute
* No validation or sanitization is performed ❌
* jQuery is used as the sink, making the DOM manipulation vulnerable

### ⚠️ Why This Is Dangerous

When user input is inserted into an `href` attribute, attackers can supply a `javascript:` URL instead of a normal path. When the victim clicks the link, the browser executes the injected JavaScript 💥.

---

## 🪜 Step-by-Step Walkthrough

### 📝 Step 1: Access the Submit Feedback Page

Navigate to the **Submit Feedback** functionality in the lab application.

This page contains:

* A feedback form 🗒️
* A **“Back”** link that redirects users after submission

---

### ✍️ Step 2: Submit Any Feedback

Enter any text into the feedback form and submit it. The content of the feedback does not matter.

After submission, observe the URL in the browser’s address bar 🔍.

---

### 🧪 Step 3: Identify the Vulnerable Parameter

In the redirected URL, notice a parameter similar to:

```
returnPath=/some/page
```

This parameter is later used by JavaScript on the page to set the `href` attribute of the **“Back”** link.

---

### 🛠️ Step 4: Inspect the Source Code (Optional but Recommended)

Using **View Page Source** or **Developer Tools**, locate the JavaScript logic that:

* Reads data from `location.search`
* Uses jQuery to select the anchor element
* Updates the `href` attribute dynamically

This confirms that `location.search` is the **source** 🧩 and `href` is the **sink**.

---

### 💉 Step 5: Craft the XSS Payload

Replace the value of the `returnPath` parameter with a `javascript:` payload:

```
javascript:alert(document.cookie)
```

Example:

```
?returnPath=javascript:alert(document.cookie)
```

---

### 🚀 Step 6: Trigger the Payload

Once the modified URL is loaded:

1. Click the **“Back”** link
2. The browser executes the JavaScript payload
3. An alert box displaying `document.cookie` appears 🍪

✅ **The lab is successfully solved!**

---

## 🧩 Root Cause Analysis

* **Source:** `location.search`
* **Sink:** jQuery `.attr("href", userInput)`
* **Missing Controls:** Input validation and URL scheme restriction

---

## 🛡️ Security Takeaways

* Never trust data from `location.search`
* Avoid dynamically assigning `href` values without validation
* Disallow dangerous URL schemes such as `javascript:` 🚫
* Use allowlists for safe redirect paths ✅

---

## 🏁 Conclusion

This lab highlights how unsafe DOM manipulation can introduce **serious XSS vulnerabilities** when user input is not properly sanitized. DOM-based XSS is particularly dangerous because it often bypasses server-side protections and relies entirely on client-side logic.

Understanding how **sources**, **sinks**, and **execution contexts** interact is essential for identifying and preventing XSS vulnerabilities 🔐.
