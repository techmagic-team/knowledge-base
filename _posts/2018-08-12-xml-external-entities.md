---
date: 2018-08-12
title: XML External Entities
categories:
  - security
description:
type: Document
set: security
set_order: 10
---

## General information

XML External Entity (XXE) refers to a specific type of Server-side Request Forgery (SSRF) attack, whereby an attacker is able
to cause Denial of Service (DoS) and access local or remote files and services, by abusing a widely available, rarely used feature
in XML parsers. XML is a vastly used data format found in everything from web services (XML-RPC, SOAP, REST…) to documents
(XML, HTML, DOCX) and image files (SVG, EXIF data…) use XML.

## Preventing

* Disable Parsing of Inline DTDs.
* Limit the Permissions of Your Web Server Process.
* Care about file processing in your application.

## Basic examples

Example of vulnerable *.xml document that can be used for attack.

```
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [ <!ELEMENT foo ANY>
<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<creds>
    <user>&xxe;</user>
    <pass>mypass</pass>
</creds>
```

## What's next?

Here are some useful links to expand your knowledge:
* Extended info about vulnerability is listed on [OWASP article](https://www.owasp.org/index.php/XML_External_Entity_(XXE)_Processing);
* [Article](https://depthsecurity.com/blog/exploitation-xml-external-entity-xxe-injection) about exploitation of this vulnerability;
* [More](https://www.sans.org/reading-room/whitepapers/application/hands-on-xml-external-entity-vulnerability-training-module-34397) about vulnerability and how to prevent it;
* [Acunetix article](https://www.acunetix.com/blog/articles/xml-external-entity-xxe-vulnerabilities/) with lot of practical examples;
* [Protection](https://www.hacksplaining.com/prevention/xml-external-entities) techniques with examples for popular programming languages.
