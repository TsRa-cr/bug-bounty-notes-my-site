# Stored XSS on My Website

## Target
My own website (self-hosted)

## Vulnerability Type
Stored XSS

---

## 1. Entry Point

User comment field:

POST /api/comment

Parameter:
content=

---

## 2. Root Cause

User input is stored and rendered without proper HTML escaping.

---

## 3. Exploitation Process

1. Submit payload:

<script>alert(1)</script>

2. Backend stores raw input

3. Frontend renders directly using innerHTML

4. Payload executes on page load

---

## 4. Proof of Concept

Payload:

<script>alert(document.domain)</script>

---

## 5. Impact

- Session hijacking
- Cookie theft
- Arbitrary JS execution

---

## 6. Fix

- Use HTML escaping
- Avoid innerHTML
- Use textContent instead

---

## 7. Lessons Learned

- Never trust user input
- Rendering layer is high-risk
