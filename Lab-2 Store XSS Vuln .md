# 🧪 Lab 2: Stored XSS into HTML Context (Nothing Encoded)

## 📌 Overview

This lab demonstrates a **Stored Cross-Site Scripting (XSS)** vulnerability in the **comment functionality** of a blog application.

Unlike one-time attacks, **stored XSS stays permanently on the server** 💾. Every time a user views the affected page, the malicious script executes automatically ⚠️, making this type of vulnerability extremely dangerous.

---

## 🎯 Lab Objective

To solve this lab, you must:

> ✅ **Submit a comment that executes the `alert()` function when the blog post is viewed.**

If the alert pops up, the lab is successfully completed 🎉.

---

## 🧠 Understanding the Vulnerability

- The application allows users to submit comments 🗨️
- Comments are **stored directly in the database** 📂
- Stored comments are displayed **without any HTML encoding or filtering** ❌
- This allows attackers to inject and execute JavaScript 💥

---

## 🔍 Key Observation

Before exploiting the issue, it’s important to understand **where the vulnerability exists**:

- 🔎 The **search feature** reflects input temporarily  
- 🔄 On page refresh, search input disappears  
- ❌ This means search input is **not stored on the server**

👉 The real vulnerability exists in the **comment section**, not the search feature.

---

## 🪜 Step-by-Step Walkthrough

### 🥇 Step 1: Open the Blog Post
- Visit the target website 🌐
- Open any blog post with a comment section 📄

---

### 🥈 Step 2: Locate the Comment Form
- Scroll down to the comments section ⬇️
- You will see a field to submit a new comment ✍️

---

### 🥉 Step 3: Inject the XSS Payload
Enter the following payload in the comment box:

```html
<script>alert(12)</script>
🧩 Payload Explanation:

<script> → Executes JavaScript

alert(12) → Visual proof of execution

🏁 Step 4: Submit the Comment

Submit the comment normally ✅

The application stores it without sanitization 🚫

🔁 Step 5: Trigger the Stored XSS

Refresh the page 🔄

Or open the blog post again 👀

🚨 An alert box appears automatically, confirming stored XSS execution.

❓ Why This Works

User input is not encoded ❌

JavaScript is embedded directly into the HTML 🧬

Browser executes the script when the page loads ⚙️
