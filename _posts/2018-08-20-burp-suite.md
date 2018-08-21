---
date: 2018-08-20
title: Burp Suite
categories:
  - security
description:
type: Document
set: security
set_order: 17
---

## General information

In last article we considered OWASP ZAP vulnerability scanner. In this article we are going to take a look at Burp Suite tool that has
a lot of interesting features and is widely used by security professionals and penetration testers around the world.

Burp or Burp Suite is a graphical tool for testing Web application security. The tool is written in Java and developed by PortSwigger
Security. The tool has two versions: a free version that can be downloaded free of charge (Free Edition) and a full version that can
be purchased after a trial period (Professional Edition). The free version has significantly reduced functionality. It was developed
to provide a comprehensive solution for web application security checks. In addition to basic functionality, such as proxy server,
scanner and intruder, the tool also contains more advanced options such as a spider, a repeater, a decoder, a comparer, an extender
and a sequencer. [Full article](https://en.wikipedia.org/wiki/Burp_suite).

## Tools

* HTTP Proxy - It operates as a web proxy server, and sits as a man-in-the-middle between the browser and destination web servers. This allows the interception, inspection and modification of the raw traffic passing in both directions.
* Scanner - A web application security scanner, used for performing automated vulnerability scans of web applications.
* Intruder - This tool can perform automated attacks on web applications. The tool offers a configurable algorithm that can generate malicious HTTP requests. The intruder tool can test and detect SQL Injections, Cross Site Scripting, parameter manipulation and vulnerabilities susceptible to brute-force attacks.
* Spider - A tool for automatically crawling web applications. It can be used in conjunction with manual mapping techniques to speed up the process of mapping an application's content and functionality.
* Repeater - A simple tool that can be used to manually test an application. It can be used to modify requests to the server, resend them, and observe the results.
* Decoder - a tool for transforming encoded data into its canonical form, or for transforming raw data into various encoded and hashed forms. It is capable of intelligently recognizing several encoding formats using heuristic techniques.
* Comparer - A tool for performing a comparison (a visual "diff") between any two items of data.
* Extender - allows the security tester to load Burp extensions, to extend Burp's functionality using the security testers own or third-party code (BAppStore)
* Sequencer - a tool for analyzing the quality of randomness in a sample of data items. It can be used to test an application's session tokens or other important data items that are intended to be unpredictable, such as anti-CSRF tokens, password reset tokens, etc.

[Full article](https://en.wikipedia.org/wiki/Burp_suite)

## What's next?

Here are some useful links to expand your knowledge and configure Burp Suite for usage on your project:
* Product [home page](https://portswigger.net/burp);
* Full documentation presented [here](https://portswigger.net/burp/help);
* [Tutorial](https://securitytraning.com/burp-suite-tutorial/) which contains explanation of all basic features and configuration examples;
* [One more](https://resources.infosecinstitute.com/burpsuite-tutorial/#gref) Burp Suite tutorial;
* [Introduction](https://www.computerweekly.com/tutorial/Burp-Suite-Guide-Part-I-Basic-tools) to Burp suite;
* Burp Suite [Udemy course](https://blog.udemy.com/burp-suite-tutorial/).
