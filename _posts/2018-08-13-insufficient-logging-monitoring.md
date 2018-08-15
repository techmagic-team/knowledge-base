---
date: 2018-08-13
title: Insufficient loging and monitoring
categories:
  - security
description:
type: Document
set: security
set_order: 15
---

## General information

Insufficient logging and monitoring, coupled with missing or ineffective integration with incident response,
allows attackers to further attack systems, maintain persistence, pivot to more systems, and tamper, extract,
or destroy data.

## Cybersecurity Monitoring Challenges

* Identify where best to employ the security budget
* Identify irregular system’s behavior, distinguishing that from the noise
* Identify that an event is really a security incident
* Connect threat intelligence with log outputs to offer an entire picture of the threat profile
* Validate the time/cost to deal with the rare security incidents

## Basic examples

An open source project forum software run by a small team was hacked using a flaw in its software. The attackers
managed to wipe out the internal source code repository containing the next version, and all of the forum contents.
Although source could be recovered, the lack of monitoring, logging or alerting led to a far worse breach. The forum
software project is no longer active as a result of this issue.

More detailed info [here](https://hdivsecurity.com/owasp-insufficient-logging-and-monitoring).

## What's next?

Here are some useful links to expand your knowledge:
* Extended info about vulnerability is listed on [OWASP article](https://www.owasp.org/index.php/Top_10-2017_A10-Insufficient_Logging%26Monitoring);
* [Article](https://www.hack2secure.com/blogs/insufficient-logging-and-monitoring--a-brief-walk-through) about prevention of this vulnerability;
* [More](https://hdivsecurity.com/owasp-insufficient-logging-and-monitoring) about this vulnerability.
