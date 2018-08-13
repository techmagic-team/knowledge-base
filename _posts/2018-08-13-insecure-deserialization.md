---
date: 2018-08-13
title: Insecure deserialization
categories:
  - security
description:
type: Document
set: security
set_order: 13
---

## General information

Insecure deserialization often leads to remote code execution. Even if deserialization flaws do not result in remote
code execution, they can be used to perform attacks, including replay attacks, injection attacks, and privilege escalation attacks.
It also occupies the #8 spot in the OWASP Top 10 2017 list.

* Serialization refers to a process of converting an object into a format which can be persisted to disk (for example,
saved to a file or a datastore), sent through streams (for example stdout), or sent over a network. The format in which
an object is serialized into, can either be binary or structured text (for example XML, JSON YAML…). JSON and XML are two
of the most commonly used serialization formats within web applications.

* Deserialization, on the other hand, is the opposite of serialization, that is, transforming serialized data coming from
a file, stream, or network socket into an object.

More information you can find in [this](https://dzone.com/articles/what-is-insecure-deserialization) article.

## Basic examples

The simplest way to load a YAML file using the PyYAML library in Python is by calling  yaml.load(). 
The following is a simple, unsafe example that loads a YAML file and parses it.
```
# Import the PyYAML dependency
import yaml
# Open the YAML file
with open('malicious.yml') as yaml_file:
# Unsafely deserialize the contents of the YAML file
contents = yaml.load(yaml_file)
# print the contents of the key 'foo' in the YAML file
print(contents['foo'])
```

Unfortunately, yaml.load() is not a safe operation, and could easily result in code execution if the attacker supplies a YAML file similar to the following.

```
foo: !!python/object/apply:subprocess.check_output ['whoami']
```

More detailed explanation of this example listed on [DZone article](https://dzone.com/articles/what-is-insecure-deserialization).

## What's next?

Here are some useful links to expand your knowledge:
* Extended info about vulnerability is listed on [OWASP article](https://www.owasp.org/index.php/Deserialization_of_untrusted_data);
* [Article](https://blog.talosintelligence.com/2017/03/apache-0-day-exploited.html) about exploitation of this vulnerability;
* HackerOne [article](http://www.hackeone.info/2017/12/a8-insecure-deserialization-2017.html) with lot of examples;
* [More](https://prakharprasad.com/shopify-remote-code-execution/) about this vulnerability.
