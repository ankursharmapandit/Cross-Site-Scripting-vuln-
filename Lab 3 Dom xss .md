# 🛡️ DOM XSS in `innerHTML` Sink Using `location.search`

## 📖 Introduction

This repository documents the solution and analysis of a **DOM-based Cross-Site Scripting (XSS)** vulnerability found in a web application's **search blog functionality**.  
The lab highlights how unsafe client-side JavaScript can lead to serious security issues when user input is directly written to the DOM.

DOM XSS vulnerabilities are particularly dangerous because they:
- Occur entirely on the client side
- Bypass many server-side security controls
- Are often overlooked during traditional testing

---

## 🎯 Lab Objective

The goal of this lab is to:

> **Exploit a DOM-based XSS vulnerability to execute JavaScript that triggers the `alert()` function.**

Successfully triggering the alert confirms that arbitrary JavaScript execution is possible in the victim’s browser.

---

## ⚠️ Vulnerability Summary

The application is vulnerable due to the following reasons:

- User input is read directly from the URL using `location.search`
- The input is written into the page using `innerHTML`
- No input validation, sanitization, or output encoding is applied

### 🔗 Source and Sink Mapping

| Component | Description |
|--------|------------|
| **Source** | `location.search` |
| **Sink** | `innerHTML` |
| **Vulnerability Type** | DOM-based XSS |

Because `innerHTML` interprets HTML markup, any injected HTML or event handler is executed by the browser.

---

## 🧪 Step 1: Interact With the Search Feature

1. Locate the search functionality on the page
2. Enter a simple test value:

3. Submit the search request

✅ This step confirms that the application reflects user input somewhere in the DOM.

---

## 🔍 Step 2: Analyze the Client-Side Code

Next, inspect how the application processes the input:

1. Open **View Page Source** or **Developer Tools**
2. Search for the reflected input (`hello`)
3. Observe how it is inserted into the page

You will find JavaScript similar to:

```javascript
document.getElementById('search-message').innerHTML = ...
## 🔴 Vulnerability Confirmation

This analysis confirms the presence of a DOM-based XSS vulnerability due to the following observations:

- Data is taken directly from `location.search`
- The data is injected into the DOM using `innerHTML`
- No sanitization or output encoding is applied

➡️ This represents a clear and exploitable vulnerable code path.

---

## 🚧 Step 3: Identify Injection Constraints

During testing, the following constraints were identified:

- `<script>` tags are not usable or may be filtered
- HTML attributes and event handlers are still allowed

👉 As a result, **event-based payloads** are the most effective method for exploitation.

---

## 💣 Step 4: Build the XSS Payload

The following payload is used to exploit the vulnerability:

```html
<img src=x onerror=alert(1)>
hy This Payload Works

The browser attempts to load the image x

The image does not exist, triggering a load error

The onerror event handler executes

JavaScript inside onerror runs

alert(1) is successfully called

✔️ No <script> tag is required
✔️ Works reliably in innerHTML sinks

▶️ Step 5: Exploit the Vulnerability

Inject the payload into the URL search parameter:

?search=<img src=x onerror=alert(1)>

🔄 Execution Flow

Browser loads the page

JavaScript reads input from location.search

Input is written to the DOM via innerHTML

Malicious HTML is parsed by the browser

Image loading fails

onerror event fires

Injected JavaScript executes
