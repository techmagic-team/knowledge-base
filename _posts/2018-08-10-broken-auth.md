---
date: 2018-08-10
title: Broken Authentication
categories:
  - security
description:
type: Document
set: security
set_order: 8
---

## General information

Application functions related to authentication and session management are often implemented incorrectly,
allowing attackers to compromise passwords, keys, or session tokens, or to exploit other implementation flaws to
assume other users' identities temporarily or permanently.

## Preventing

* Require strong passwords
* Rate-limit maximum login attempts allowed per user ID or IP for a given time period
* Lockout mechanism, preventing a user from logging in upon multiple failed attempts
* Console Log all login failures with metadata (such as IP address) and lockouts, and actively monitor it
* Secure password recover requires the old password secret question prevert simple number, character
* Ensure that the forgot password and other recovery paths do not reveal the current password

## Basic examples

An e-commerce application supports URL rewriting, putting session IDs in the URL −
http://example.com/sale/saleitems/jsessionid=2P0OC2JSNDLPSKHCJUN2JV/?item=laptop
An authenticated user of the site forwards the URL to their friends to know about the discounted sales.
He e-mails the above link without knowing that the user is also giving away the session IDs. 
When his friends use the link, they use his session and credit card.

## What's next?

Here are some useful links to expand your knowledge:
* Extended info about vulnerability is listed on [OWASP article](https://www.owasp.org/index.php/Session_Management_Cheat_Sheet#Session_ID_Properties);
* How to test this vulnerability you can find [here](https://www.tutorialspoint.com/security_testing/testing_broken_authentication.htm);
* [Another example](https://github.com/wahengchang/nodejs-security-must-know/tree/master/broken_authentication);
* Session management [example](https://dzone.com/articles/owasp-broken-authentication);
* [Article](https://hdivsecurity.com/owasp-broken-authentication-and-session-management) with provided vulnerability explanation and examples.
