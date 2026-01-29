# 🔬 Lab-10 DOM XSS in `document.write` Sink Using `location.search` Inside a `<select>` Element

## 📌 Overview

This lab demonstrates a **DOM-based Cross-Site Scripting (XSS)** vulnerability in the **stock checker functionality** of the application 🧨. The issue occurs because the application uses the JavaScript `document.write()` function to write data directly into the page.

The value written to the page is taken from **`location.search`**, which is fully attacker-controlled via the URL. This data is inserted **inside a `<select>` element** without proper sanitization, allowing an attacker to break out of the HTML context and execute arbitrary JavaScript.

---

## 🎯 Lab Objective

To successfully solve this lab, you must:

* Exploit the DOM-based XSS vulnerability in the stock checker
* Break out of the `<select>` element
* Execute the `alert()` function ⚠️

---

## 🧠 Vulnerability Explanation

### 🔑 Key Observations

* User input is read from `location.search`
* The input is written to the DOM using `document.write()` ❌
* The output is embedded inside a `<select>` element
* No input validation or output encoding is applied

### ⚠️ Why This Is Dangerous

The `document.write()` function directly injects raw HTML into the page. When combined with user-controlled input, this creates a **high-risk DOM XSS sink**. By closing the existing HTML context, an attacker can inject script tags that execute immediately 💥.

---

## 🪜 Step-by-Step Walkthrough

### 📝 Step 1: Open the Vulnerable Page

Navigate to the website and open any product or post page. Scroll down until you see the **Check stock** feature.

---

### 🏬 Step 2: Use the Stock Checker Normally

Select a location such as:

* **London (675)**

Then click **Check stock** to observe normal behavior.

---

### 🧪 Step 3: Capture the Request (Optional but Helpful)

Using **Burp Suite**, intercept the stock check request and observe the request parameters. You will notice a parameter similar to:

```
StoreID=675
```

This value is later reflected into the page using `document.write()`.

---

### 💉 Step 4: Identify the Injection Point

The application reads the `StoreID` value from the URL (`location.search`) and writes it inside a `<select>` element. This allows you to supply arbitrary HTML via the URL parameter.

---

### 🧨 Step 5: Craft the XSS Payload

Replace the `StoreID` parameter with the following payload:

```
<script>alert(1)</script>
```

Example:

```
&StoreID=<script>alert(1)</script>
```

---

### 🚀 Step 6: Execute the Payload

1. Load the modified URL in the browser
2. The injected script breaks out of the `<select>` element
3. The JavaScript `alert(1)` function executes immediately ⚡

✅ **The lab is successfully solved!**

---

## 🧩 Root Cause Analysis

* **Source:** `location.search`
* **Sink:** `document.write()`
* **Context:** HTML injection inside a `<select>` element
* **Missing Controls:** Input validation and output encoding

---

## 🛡️ Security Takeaways

* Never use `document.write()` with user-controlled input 🚫
* Treat URL parameters as untrusted data
* Use safe DOM APIs such as `textContent`
* Apply strict output encoding based on context 🔐

---

## 🏁 Conclusion

This lab highlights how dangerous **DOM XSS vulnerabilities** can arise when unsafe JavaScript functions like `document.write()` are combined with attacker-controlled input. Breaking out of HTML elements such as `<select>` tags allows attackers to inject and execute arbitrary scripts.

Understanding **DOM sinks**, **HTML contexts**, and **client-side data flows** is essential for identifying and preventing DOM-based XSS vulnerabilities 🚨.
