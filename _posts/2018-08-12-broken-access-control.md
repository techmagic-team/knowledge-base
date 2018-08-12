---
date: 2018-08-12
title: Broken Access Control
categories:
  - security
description:
type: Document
set: security
set_order: 11
---

## General information

Restrictions on what authenticated users are allowed to do are often not properly enforced. Attackers can exploit
these flaws to access unauthorized functionality and/or data, such as access other users' accounts, view sensitive
files, modify other users' data, change access rights, etc.


## Access Control Types with examples

The access controls can be implemented in three different levels. They are:
* Physical Access Controls – includes physical access limitations on what an authenticated user can do on a resource.
Example: Locks, workplace separation
* Logical Access Controls - includes various software measures, biometric security features and sophisticated
password programs to enforce access controls. Example: Encryption, passwords, tokens
* Administrative Access Control – Here, the policies and procedures are defined by the organization to implement
entire access control. Example: policies & procedures, awareness training

More information presented in [this article](https://www.hack2secure.com/blogs/understanding-broken-access-control-risk).

## Basic examples

When the access controls are defective, an intruder can compromise the whole application, taking control of admin functionality and misusing
sensitive data that they are unauthorized to access. Here is the list of vulnerabilities that allow these changes to the attackers:
* Bypassing verification of access control by changing the URL, the HTML page, internal application state or by using an attack tool.
* Not restricting the others from viewing or modifying someone else’s record or account.
* Privilege escalation- Acting as an administrator when logged in as another user.
* Metadata manipulation with tampering or replaying to elevate privileges.
* CORS (Cross-Origin Resource Sharing) misconfiguration permits unauthorized access to the API.

## What's next?

Here are some useful links to expand your knowledge:
* Extended info about vulnerability is listed on [OWASP article](https://www.owasp.org/index.php/Broken_Access_Control);
* [Article](https://blog.detectify.com/2018/04/10/owasp-top-10-broken-access-control/) about exploitation and detection of this vulnerability;
* [More](https://www.hacksplaining.com/prevention/broken-access-control) about vulnerability and how to prevent it.
