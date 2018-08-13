---
date: 2018-08-13
title: Using vulnerable components
categories:
  - security
description:
type: Document
set: security
set_order: 14
---

## General information

Components, such as libraries, frameworks, and other software modules, run with the same privileges as the application.
If a vulnerable component is exploited, such an attack can facilitate serious data loss or server takeover.
Some vulnerable components (e.g., framework libraries) can be identified and exploited with automated tools, expanding the threat
agent pool beyond targeted attackers to include chaotic actors.

## Prevention

* Identify all components and the versions you are using, including all dependencies. (e.g., the versions plugin).
* Monitor the security of these components in public databases, project mailing lists, and security mailing lists,
and keep them up to date.
* Establish security policies governing component use, such as requiring certain software development practices,
passing security tests, and acceptable licenses.
* Where appropriate, consider adding security wrappers around components to disable unused functionality and/ or secure
weak or vulnerable aspects of the component.

More detailed information in [OWASP article](https://www.owasp.org/index.php/Top_10_2013-A9-Using_Components_with_Known_Vulnerabilities).

## Basic examples

* adm-zip lib vulnerability

Affected versions of the package are vulnerable to Arbitrary File Write via Archive Extraction (AKA "Zip Slip").
It can be exploited using a specially crafted zip archive, that holds path traversal filenames. When exploited,
a filename in a malicious archive is concatenated to the target extraction directory, which results in the final path
ending up outside of the target folder. For instance, a zip may hold a file with a ../../file.exe location and thus break
out of the target folder. If an executable or a configuration file is overwritten with a file containing malicious code,
the problem can turn into an arbitrary code execution issue quite easily.

More detailed info [here](https://snyk.io/vuln/npm:adm-zip:20180415).

## What's next?

Here are some useful links to expand your knowledge:
* Extended info about vulnerability is listed on [OWASP article](https://www.owasp.org/index.php/Top_10_2013-A9-Using_Components_with_Known_Vulnerabilities);
* [Article](https://www.c-sharpcorner.com/blogs/using-components-with-known-vulnerabilities-and-their-prevention-mechanisms) about prevention of this vulnerability;
* GitHub [article](https://help.github.com/articles/about-security-alerts-for-vulnerable-dependencies/) about vulnerable dependencies;
* [More](https://hdivsecurity.com/docs/using-components-with-known-vulnerabilities/) about this vulnerability.

To prevent this type of vulnerabilities please take a look at [Snyk](https://snyk.io/) tool. Link for quick [start](https://snyk.io/docs/using-snyk/)
with Snyk and Node.js
