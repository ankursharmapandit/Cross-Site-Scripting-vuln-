# 🛡️ lab-3 DOM-Based Vulnerability: DOM XSS Using `document.write` and `location.search`

## 📖 Introduction

**DOM-based Cross-Site Scripting (DOM XSS)** is a web security vulnerability where malicious JavaScript code executes directly in a user’s browser due to **unsafe client-side JavaScript**.  
Unlike reflected or stored XSS, DOM XSS does **not originate from the server response** but from how the page’s JavaScript processes user-controlled input.

Attackers can inject harmful code through inputs such as:
- URL parameters
- Search bars
- Comment boxes

If the application reflects this input back into the page **without proper sanitization**, the injected code executes in every user’s browser.

---

## 🧠 Core Concepts of DOM-Based XSS

There are **two important components** in any DOM-based XSS vulnerability:

### 1️⃣ Source

A **source** is where untrusted user input enters the application.

Common DOM XSS sources include:
- `location.search`
- `location.href`
- `document.URL`
- `document.referrer`

In DOM-based XSS, attackers fully control these values through the browser URL.

---

### 2️⃣ Sink

A **sink** is a JavaScript function that executes or processes the input in a dangerous way.

Common DOM XSS sinks include:
- `innerHTML`
- `document.write`
- `eval`
- `setTimeout`
- `setInterval`

⚠️ When untrusted input flows from a **source** into a **sink**, a DOM XSS vulnerability exists.

---

## 🧪 Lab 3: DOM XSS in `document.write` Sink Using Source `location.search`

### 📌 Lab Description

This lab contains a **DOM-based Cross-Site Scripting (XSS)** vulnerability in the **search query tracking functionality**.

The vulnerability exists because:
- The application uses the JavaScript `document.write()` function
- `document.write()` writes data directly to the page
- The data passed comes from `location.search`
- The input is fully controllable through the website URL
- No sanitization or encoding is applied

### 🎯 Objective

> Exploit the vulnerability to execute JavaScript that calls the `alert()` function.

---

## 🧪 Step 1: Interact With the Application

1. Open the vulnerable website
2. Locate the **search bar**
3. Enter a simple test value:

```text
hello
Submit the search request

This confirms that the search input is reflected on the page.

🔍 Step 2: Inspect the Client-Side Code

Open Browser Developer Tools

Inspect the page source

Search for the value hello

You will notice:

The value appears more than once

The input is written directly into the page

One of the locations uses document.write()

This confirms that the application dynamically writes user input into the DOM.

🔴 Vulnerability Confirmation

This confirms the vulnerable code path:

Data is taken directly from location.search

The data is injected into the DOM using document.write

No sanitization or output encoding is applied

➡️ This creates a direct DOM-based XSS vulnerability.

🚧 Step 3: Identify Injection Constraints

During testing, the following behavior is observed:

<script> tags are allowed

Input is written directly into the page context

No filtering or escaping is applied

👉 This allows direct JavaScript execution via injected payloads.

💣 Step 4: Build the XSS Payload

The following payload is used to exploit the vulnerability:

<img src="/resources/images/tracker.gif?searchTerms='"><script>alert(1)</script>//';

🧠 Why This Payload Works

The injected input breaks out of the intended HTML structure

A <script> tag is successfully injected

The browser parses the injected HTML

JavaScript inside the <script> tag executes

alert(1) is successfully called

✔️ Direct script execution
✔️ Fully supported by document.write

▶️ Step 5: Execute the Payload

Inject the payload via the URL search parameter.
Example
?search=<img src="/resources/images/tracker.gif?searchTerms='"><script>alert(1)</script>//';

🔄 Execution Flow

Browser loads the page

JavaScript reads input from location.search

Input is passed to document.write

Malicious HTML is parsed

<script> tag executes

JavaScript runs successfully

🎉 An alert box appears — the lab is successfully solved.
