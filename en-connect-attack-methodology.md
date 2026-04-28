# Connect Website Security Testing Methodology

This document describes the methodology used to analyze security issues on my own website (Connect).  
The goal is not exploitation, but to build a structured approach to vulnerability discovery.

---

## Core Principle

Before testing any input:

**Map → Identify → Trace → Verify**

Do NOT randomly test inputs.  
Always understand how data flows through the system first.

---

## 1. Objective

The purpose of this exercise is to develop the ability to:

1. Identify input points
2. Trace data flow
3. Understand validation mechanisms
4. Evaluate real impact

---

## 2. Test Environment Setup

To avoid damaging production data:

- Use a local or test environment
- Backup database before testing
- Avoid direct testing on live system

---

## 3. Attack Surface Mapping

### 3.1 Page Enumeration

All accessible pages are listed:

- Home
- Login
- Register
- Profile
- Edit Profile
- Search
- Post Creation
- Comment Section
- Messaging
- File Upload
- Password Reset
- Admin Panel
- API Endpoints

---

### 3.2 Input Points Identification

Every user-controlled input is recorded:

- Form fields (username, password, bio, etc.)
- URL parameters (`?id=`, `?q=`)
- POST body parameters
- Cookies
- HTTP headers
- File uploads
- Hidden fields

Important mindset:

**Input is not just visible fields — it includes all user-controllable data.**

---

### 3.3 Data Flow Mapping

For each input, determine:

**Where does it go?**

Examples:

- Username → Profile page
- Bio → User profile display
- Comment → Post page
- Search query → Search results page
- File name → Download list
- Message → Chat interface

---

## 4. Vulnerability Discovery Models

### Model 1: Input → Output

Used for:

- XSS
- HTML Injection

Key questions:

- Is input reflected?
- Is it escaped?
- In which context is it rendered?

---

### Model 2: Input → Database Query

Used for:

- SQL Injection
- Logic bypass

Key questions:

- Does input affect queries?
- Does behavior change with abnormal input?

---

### Model 3: Input → File System

Used for:

- File upload vulnerabilities
- Path traversal

Key questions:

- Does input become file name or path?

---

### Model 4: Input → Authorization Logic

Used for:

- IDOR
- Privilege escalation

Key questions:

- Can IDs be modified to access other users’ data?
- Is authorization enforced server-side?

---

## 5. Initial Testing Strategy

Focus areas:

1. XSS
2. SQL-related anomalies
3. Authorization issues
4. File upload handling
5. Information disclosure

---

## 6. XSS Testing Approach

### Step 1: Identify Reflection Points

- Username display
- Bio
- Comments
- Search results
- Messages
- File names

---

### Step 2: Baseline Testing

Use marker strings:
test_marker_12345

Purpose:

- Track data flow
- Identify reflection locations

---

### Step 3: HTML Interpretation Check

Determine if input is:

- Rendered as text
- Interpreted as HTML

---

### Step 4: Context Analysis

Identify where input appears:

- HTML body
- Attribute values
- JavaScript context
- URL context

---

### Step 5: Source Inspection

Check:

- Character escaping (`<`, `"`)
- DOM structure
- Rendering method (`innerHTML` vs `textContent`)

---

## 7. SQL Injection Thinking (Conceptual)

Focus on behavior changes:

- Errors
- Different responses
- Timing anomalies
- Unexpected query results

Code review focus:

- String concatenation
- Non-parameterized queries

---

## 8. Authorization Testing

### Horizontal Access Control

- Modify IDs to access other users’ data

### Vertical Access Control

- Access admin endpoints as normal user

### Unauthenticated Access

- Access protected resources without login

---

## 9. File Upload Testing

Check:

- File type validation
- Storage location
- Filename handling
- Execution risk

---

## 10. Information Disclosure

Look for:

- Debug messages
- Stack traces
- Hidden endpoints
- Exposed configuration
- Backup files
- `.git` exposure

---

## 11. Documentation Template

Each test case is recorded as:
[Feature]
[Input Point]
[Request]
[Parameter]
[Input]
[Reflection]
[Behavior]
[Security Hypothesis]
[Next Step]

---

## 12. Key Insight

The goal is NOT to memorize payloads.

The goal is to build the ability to think:

- Where does input go?
- How is it processed?
- What system component handles it?

**Payloads are tools.  
Thinking is the skill.**
