---
date: 2018-03-20
title: General Web Security Info
categories:
  - security
description:
type: Document
set: security
set_order: 1
---

## General Web Security Info

There are different types of security vulnerabilities in WEB, but OWASP organization maintains TOP 10 of them in their [OWASP Top 10 Project](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project). They review it once 3-4 years and update the doc so check for updates on their site.

###### In 2017 the top 10 security risks are:
* A1:2017 - Injection
* A2:2017 - Broken Authentication
* A3:2017 - Sensitive Data Exposure
* A4:2017 - XML External Entities (XXE)
* A5:2017 - Broken Access Control
* A6:2017 - Security Misconfiguration
* A7:2017 - Cross-Site Scripting (XSS)
* A8:2017 - Insecure Deserialization
* A9:2017 - Using Components with Known Vulnerabilities
* A10:2017 - Insufficient Logging & Monitoring

###### Resources where you can find the most up-to-date info and answers about web security:
* [OWASP](https://www.owasp.org/index.php/Main_Page)
* [MDN Web Security](https://developer.mozilla.org/en-US/docs/Web/Security)
* [StackExchange Internet Security](https://security.stackexchange.com/)

Note: Without reading next articles **don't start working on security** of your app, unless you know what you're doing.

###### Key points to undestand before working on your web app security:
* [Mixed Content (HTTP + HTTPS)](https://developer.mozilla.org/en-US/docs/Web/Security/Mixed_content) - if your web app is servedthrough HTTPS you shouldn't have any other content on the page served through HTTP (scripts, images, fonts etc.)
* [Same Origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) - MUST UNDERSTAND
    * The same-origin policy restricts how a document or script loaded from one origin can interact with a resource from another origin. **It is a critical security mechanism** for isolating potentially malicious documents.
* [Secure context](https://developer.mozilla.org/en-US/docs/Web/Security/Secure_Contexts)
* [TLS (HTTP over TLS)](https://developer.mozilla.org/en-US/docs/Web/Security/Transport_Layer_Security)
* [HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) - save it as a bookmark so you can read about any http header and whats it all about

