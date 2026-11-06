# Modern Front-End Security Concepts

## Project Overview

This project focuses on identifying and fixing five common front-end security vulnerabilities in a User Security Dashboard built using JavaScript, Node.js, and Express.js.

## Technologies Used

- JavaScript
- Node.js
- Express.js
- Helmet.js
- HTML and CSS

## Setup Instructions

```bash
npm install
npm run dev
```

Open the application at `http://localhost:3000`.

## Vulnerabilities Fixed

### 1. DOM-Based XSS
- **Issue:** User input from URL parameters was directly inserted into the DOM using `innerHTML`.
- **Fix:** Replaced `innerHTML` with safe DOM methods like `createElement()` and `textContent`.
- **Test:** Tried a malicious URL containing an image with an `onerror` event. JavaScript should not execute.

### 2. Prototype Pollution
- **Issue:** Incoming settings were copied without validating object properties.
- **Fix:** Added an allowlist for setting keys and values and used `Object.create(null)`.
- **Test:** Sent a request containing `__proto__`. The server should reject it with HTTP 400.

### 3. Content Security Policy (CSP)
- **Issue:** The application did not have proper content security restrictions.
- **Fix:** Integrated Helmet.js and configured CSP directives with a unique nonce for inline scripts.
- **Test:** Verified the `Content-Security-Policy` response header.

### 4. SSR Token Leakage
- **Issue:** The complete user object, including session and refresh tokens, was exposed to the browser.
- **Fix:** Created a public user DTO containing only `id`, `name`, `email`, and `role`. Updated the HTML response and profile API to use it.
- **Test:** Verified that `sessionToken` and `refreshToken` are absent from the API and HTML responses.

### 5. Clickjacking
- **Issue:** The application could potentially be embedded in another website using an iframe.
- **Fix:** Configured `X-Frame-Options: DENY` and CSP `frame-ancestors 'none'` using Helmet.js.
- **Test:** Created `test/clickjacking-test.html` to attempt iframe embedding. Verified that the browser blocks the dashboard.

## Testing Summary

| Vulnerability | Verification |
|---|---|
| DOM XSS | Malicious URL input |
| Prototype Pollution | Malicious settings request |
| CSP | Security response headers |
| SSR Token Leakage | API and HTML response |
| Clickjacking | Iframe embedding test |

## Git Information

**Feature Branch:** `feature/frontend-security-fixes`

## Conclusion

All five identified security vulnerabilities were addressed using appropriate security practices. The fixes improve application security while maintaining the existing dashboard functionality.