---
date: 2018-08-20
title: AWS security
categories:
  - security
description:
type: Document
set: security
set_order: 18
---

## General information

In this article we're going to talk about security of AWS services that can be used on project. This is very important in nowadays
world because more and more companies use cloud services in their projects. It's also desired target for the "bad guyz"
because they can do a lot of bad things with broken AWS accounts (e.g. cryptocurrency mining, sensitive data dumps, etc.).
With implementation of GDPR this problem became more critical due to big financial fines for personal data disclosure.

## Few words about AWS data leakages

Amazon Web Services (AWS) is a widely-used cloud computing provider for organizations of all sizes, offering convenient,
scalable hosting for development, databases and more. Amazon S3 buckets aren't the only data repositories that can leak
data because of the organization's configuration errors. Other cloud services on the AWS platform are often found accessible 
by anyone on the Internet.

Elastic Block Store (EBS) provides highly-available block-level storage volumes that are used with EC2 instances. EBS snapshots
are point-in-time backups of EC2s, used for disaster recovery solutions. For example, in the event of a ransomware attack, you
might need to quickly access a backup of your organization's data for business continuity purposes.

These snapshots are typically moved or need to be accessed elsewhere, which is where the problem lies, as AWS Security Consultant
Scott Piper, working on behalf of Duo, explained.

Some AWS users make these snapshots public so they can easily access them. As a result, Piper found 116,386 publicly available
EBS snapshots exposed to the internet, from 3,213 different accounts. [Full article](https://duo.com/decipher/exposed-aws-resources-leaked-sensitive-data)

## Major data leaks related to Amazon S3 buckets configuration mistakes

* Top defense contractor Booz Allen Hamilton leaks 60,000 files, including employee security credentials and passwords to a US government system.
* Verizon partner leaks personal records of over 14 million Verizon customers, including names, addresses, account details, and for some victims — account PINs.
* An AWS S3 server leaked the personal details of WWE fans who registered on the company's sites. 3,065,805 users were exposed.
* Another AWS S3 bucket leaked the personal details of over 198 million American voters. The database contained information from three data mining companies known to be associated with the Republican Party.
* Another S3 database left exposed only leaked the personal details of job applications that had Top Secret government clearance.
* Dow Jones, the parent company of the Wall Street Journal, leaked the personal details of 2.2 million customers.
* Omaha-based voting machine firm Election Systems & Software (ES&S) left a database exposed online that contained the personal records of 1.8 million Chicago voters.
* Security researchers discovered a Verizon AWS S3 bucket containing over 100 MB of data about the company's internal system named Distributed Vision Services (DVS), used for billing operations.
* An auto-tracking company leaked over a half of a million records with logins/passwords, emails, VIN (vehicle identification number), IMEI numbers of GPS devices and other data that is collected on their devices, customers and auto dealerships.

[Full article](https://www.bleepingcomputer.com/news/security/7-percent-of-all-amazon-s3-servers-are-exposed-explaining-recent-surge-of-data-leaks/)

## Tools

For tools that can be used for offensive|defensive testing of AWS services please take a look at [this arsenal](https://github.com/toniblyx/my-arsenal-of-aws-security-tools).
Author described lot of different easy to use free tools for increasing AWS security on project. Most of them have
good documentation and explanation of major features and use cases. 

## What's next?

Here are some useful links to expand your knowledge and improve AWS services security on your project:
* How to secure AWS like a boss [article](https://www.infoworld.com/article/3026395/security/how-to-secure-amazon-web-services-like-a-boss.html)
* Top 7 AWS security [issues](https://www.threatstack.com/blog/what-you-need-to-know-about-the-top-7-aws-security-issues)
* AWS cloud security [guide](https://aws.amazon.com/security/)
