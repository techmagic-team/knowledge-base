---
date: 2018-08-12
title: Security misconfiguration
categories:
  - security
description:
type: Document
set: security
set_order: 12
---

## General information

Security misconfiguration is the most commonly seen issue. This is commonly a result of insecure default configurations,
incomplete or ad hoc configurations, open cloud storage, misconfigured HTTP headers, and verbose error messages containing
sensitive information.

Improper server or web application configuration leading to various flaws:
* Debugging enabled.
* Incorrect folder permissions.
* Using default accounts or passwords.
* Setup/Configuration pages enabled.
* All of your data could be stolen or modified slowly over time.

## Basic examples

Directory listing is not disabled on your server. Let's consider potential risk.
Attacker discovers they can simply list directories to find any file. Attacker finds and downloads all your 
compiled Java classes, which they decompile and reverse engineer to get all your custom code. They then find a serious
access control flaw in your application.

## What's next?

Here are some useful links to expand your knowledge:
* Extended info about vulnerability is listed on [OWASP article](https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration);
* [Article](https://vitalflux.com/owasp-security-misconfiguration-classic-example-1/#prettyPhoto) about exploitation of this vulnerability;
* Great [article](https://support.portswigger.net/customer/portal/articles/1965728-using-burp-to-test-for-security-misconfiguration-issues) in which described vulnerability detection using Burp Suite;
* [More](https://hdivsecurity.com/owasp-security-misconfiguration) about vulnerability and how to prevent it.
