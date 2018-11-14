---
date: 2018-11-14
title: AWS security
categories:
  - security
description:
type: Document
set: security
set_order: 19
---

## General information

In this article we're going to talk about security of AWS services that can be used on project. This is very important in nowadays
world because more and more companies use cloud services in their projects. It's also desired target for the "bad guyz"
because they can do a lot of bad things with broken AWS accounts (e.g. cryptocurrency mining, sensitive data dumps, etc.).
With implementation of GDPR this problem became more critical due to big financial fines for personal data disclosure.

## How to Automatically Update Your Security Groups for Amazon CloudFront and AWS WAF by Using AWS Lambda

# Setup

## update-security-groups

A Lambda function for updating the **cloudfront** EC2 security group ingress rules
with the CloudFront IP range changes.


## Security Group

This Lambda function updates a total possibility of 4 EC2 security groups tagged as the following:
*  `Name: cloudfront_g` and `AutoUpdate: true` and a `Protocol` tag with value `http` or `https`.
*  `Name: cloudfront_r` and `AutoUpdate: true` and a `Protocol` tag with value `http` or `https`.

**Note:** For CloudFront to properly connect to your origin over HTTP or HTTPS only, you will need two security groups with `Name: cloudfront_g` and `Name: cloudfront_r` set for http or https depending on the protocol used. If you require both HTTP and HTTPS protocols to your origin, you will need a total of 4 security groups.

## Event Source

This lambda function is designed to be subscribed to the 
[AmazonIpSpaceChanged](http://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html#subscribe-notifications) 
SNS topic. In the _Add Event Source_ dialog, select **SNS** in the *Event source type*, and populate *SNS Topic* with `arn:aws:sns:us-east-1:806199016981:AmazonIpSpaceChanged`.


## Policy

To be able to make sufficient use of this Lambda function, you will require a role with a number of permissions. An example policy is as follows:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:RevokeSecurityGroupIngress"
            ],
            "Resource": "arn:aws:ec2:[region]:[account-id]:security-group/*"
        },
        {
            "Effect": "Allow",
            "Action": "ec2:DescribeSecurityGroups",
            "Resource": "*"
        },
        {
            "Action": [
                "logs:CreateLogGroup",
                 "logs:CreateLogStream",
                 "logs:PutLogEvents"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:logs:*:*:*"
        }
    ]
}
```

Be sure to replace `[region]` with the AWS Region for your security groups, and `[account-id]` with your account number.

For more information on ip-ranges.json, read the documentation on [AWS IP Address Ranges](http://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html).

## Create your Lambda function
Now that you have created your Lambda execution role, you are ready to create your Lambda function:

1. Go to the Lambda console and choose Create function. On the next page, choose Author from scratch. (Because I’ll be providing the code for your Lambda function, you can skip the blueprint step, but for other functions, blueprints can be a great way to get started.)
2. On the Configure triggers page, choose Next.
3. Give your Lambda function a name and description, and select Python 2.7 from the Runtime menu.
4. Paste the Lambda function code from the aws-cloudfront-samples GitHub repository. Important note: By default, Lambda configures the SDK in its own region. If the security groups are in a different region than the Lambda function, you must update the SDK client with the correct region (client = boto3.client(‘ec2’,region_name=‘yourregion’)).

<details><summary>Lambda code</summary>
<p>
```
import boto3
import hashlib
import json
import urllib2
```
### Name of the service, as seen in the ip-groups.json file, to extract information for
SERVICE = "CLOUDFRONT"
### Ports your application uses that need inbound permissions from the service for
INGRESS_PORTS = { 'Http' : 80, 'Https': 443 }
### Tags which identify the security groups you want to update
SECURITY_GROUP_TAG_FOR_GLOBAL_HTTP = { 'Name': 'cloudfront_g', 'AutoUpdate': 'true', 'Protocol': 'http' }
SECURITY_GROUP_TAG_FOR_GLOBAL_HTTPS = { 'Name': 'cloudfront_g', 'AutoUpdate': 'true', 'Protocol': 'https' }
SECURITY_GROUP_TAG_FOR_REGION_HTTP = { 'Name': 'cloudfront_r', 'AutoUpdate': 'true', 'Protocol': 'http' }
SECURITY_GROUP_TAG_FOR_REGION_HTTPS = { 'Name': 'cloudfront_r', 'AutoUpdate': 'true', 'Protocol': 'https' }
```
def lambda_handler(event, context):
    print("Received event: " + json.dumps(event, indent=2))
    message = json.loads(event['Records'][0]['Sns']['Message'])
```
    # Load the ip ranges from the url
    ip_ranges = json.loads(get_ip_groups_json(message['url'], message['md5']))
```
    # extract the service ranges
    global_cf_ranges = get_ranges_for_service(ip_ranges, SERVICE, "GLOBAL")
    region_cf_ranges = get_ranges_for_service(ip_ranges, SERVICE, "REGION")
    ip_ranges = { "GLOBAL": global_cf_ranges, "REGION": region_cf_ranges }

    # update the security groups
    result = update_security_groups(ip_ranges)
```
    return result
```
def get_ip_groups_json(url, expected_hash):
    print("Updating from " + url)
```
    response = urllib2.urlopen(url)
    ip_json = response.read()
```
    m = hashlib.md5()
    m.update(ip_json)
    hash = m.hexdigest()
```
    if hash != expected_hash:
        raise Exception('MD5 Mismatch: got ' + hash + ' expected ' + expected_hash)
```
    return ip_json
```
def get_ranges_for_service(ranges, service, subset):
    service_ranges = list()
    for prefix in ranges['prefixes']:
        if prefix['service'] == service and ((subset == prefix['region'] and subset == "GLOBAL") or (subset != 'GLOBAL' and prefix['region'] != 'GLOBAL')):
            print('Found ' + service + ' region: ' + prefix['region'] + ' range: ' + prefix['ip_prefix'])
            service_ranges.append(prefix['ip_prefix'])
```
    return service_ranges
```
def update_security_groups(new_ranges):
    client = boto3.client('ec2')
```
    global_http_group = get_security_groups_for_update(client, SECURITY_GROUP_TAG_FOR_GLOBAL_HTTP)
    global_https_group = get_security_groups_for_update(client, SECURITY_GROUP_TAG_FOR_GLOBAL_HTTPS)
    region_http_group = get_security_groups_for_update(client, SECURITY_GROUP_TAG_FOR_REGION_HTTP)
    region_https_group = get_security_groups_for_update(client, SECURITY_GROUP_TAG_FOR_REGION_HTTPS)
```
    print ('Found ' + str(len(global_http_group)) + ' CloudFront_g HttpSecurityGroups to update')
    print ('Found ' + str(len(global_https_group)) + ' CloudFront_g HttpsSecurityGroups to update')
    print ('Found ' + str(len(region_http_group)) + ' CloudFront_r HttpSecurityGroups to update')
    print ('Found ' + str(len(region_https_group)) + ' CloudFront_r HttpsSecurityGroups to update')
```
    result = list()
    global_http_updated = 0
    global_https_updated = 0
    region_http_updated = 0
    region_https_updated = 0
```
    for group in global_http_group:
        if update_security_group(client, group, new_ranges["GLOBAL"], INGRESS_PORTS['Http']):
            global_http_updated += 1
            result.append('Updated ' + group['GroupId'])
    for group in global_https_group:
        if update_security_group(client, group, new_ranges["GLOBAL"], INGRESS_PORTS['Https']):
            global_https_updated += 1
            result.append('Updated ' + group['GroupId'])
    for group in region_http_group:
        if update_security_group(client, group, new_ranges["REGION"], INGRESS_PORTS['Http']):
            region_http_updated += 1
            result.append('Updated ' + group['GroupId'])
    for group in region_https_group:
        if update_security_group(client, group, new_ranges["REGION"], INGRESS_PORTS['Https']):
            region_https_updated += 1
            result.append('Updated ' + group['GroupId'])
```
    result.append('Updated ' + str(global_http_updated) + ' of ' + str(len(global_http_group)) + ' CloudFront_g HttpSecurityGroups')
    result.append('Updated ' + str(global_https_updated) + ' of ' + str(len(global_https_group)) + ' CloudFront_g HttpsSecurityGroups')
    result.append('Updated ' + str(region_http_updated) + ' of ' + str(len(region_http_group)) + ' CloudFront_r HttpSecurityGroups')
    result.append('Updated ' + str(region_https_updated) + ' of ' + str(len(region_https_group)) + ' CloudFront_r HttpsSecurityGroups')
```
    return result
```
def update_security_group(client, group, new_ranges, port):
    added = 0
    removed = 0
```
    if len(group['IpPermissions']) > 0:
        for permission in group['IpPermissions']:
            if permission['FromPort'] <= port and permission['ToPort'] >= port :
                old_prefixes = list()
                to_revoke = list()
                to_add = list()
                for range in permission['IpRanges']:
                    cidr = range['CidrIp']
                    old_prefixes.append(cidr)
                    if new_ranges.count(cidr) == 0:
                        to_revoke.append(range)
                        print(group['GroupId'] + ": Revoking " + cidr + ":" + str(permission['ToPort']))
```
                for range in new_ranges:
                    if old_prefixes.count(range) == 0:
                        to_add.append({ 'CidrIp': range })
                        print(group['GroupId'] + ": Adding " + range + ":" + str(permission['ToPort']))
```
                removed += revoke_permissions(client, group, permission, to_revoke)
                added += add_permissions(client, group, permission, to_add)
    else:
        to_add = list()
        for range in new_ranges:
            to_add.append({ 'CidrIp': range })
            print(group['GroupId'] + ": Adding " + range + ":" + str(port))
        permission = { 'ToPort': port, 'FromPort': port, 'IpProtocol': 'tcp'}
        added += add_permissions(client, group, permission, to_add)
```
    print (group['GroupId'] + ": Added " + str(added) + ", Revoked " + str(removed))
    return (added > 0 or removed > 0)
```
def revoke_permissions(client, group, permission, to_revoke):
    if len(to_revoke) > 0:
        revoke_params = {
            'ToPort': permission['ToPort'],
            'FromPort': permission['FromPort'],
            'IpRanges': to_revoke,
            'IpProtocol': permission['IpProtocol']
        }
```
        client.revoke_security_group_ingress(GroupId=group['GroupId'], IpPermissions=[revoke_params])
```
    return len(to_revoke)
```
def add_permissions(client, group, permission, to_add):
    if len(to_add) > 0:
        add_params = {
            'ToPort': permission['ToPort'],
            'FromPort': permission['FromPort'],
            'IpRanges': to_add,
            'IpProtocol': permission['IpProtocol']
        }
```
        client.authorize_security_group_ingress(GroupId=group['GroupId'], IpPermissions=[add_params])
```
    return len(to_add)
```
def get_security_groups_for_update(client, security_group_tag):
    filters = list();
    for key, value in security_group_tag.iteritems():
        filters.extend(
            [
                { 'Name': "tag-key", 'Values': [ key ] },
                { 'Name': "tag-value", 'Values': [ value ] }
            ]
        )
```
    response = client.describe_security_groups(Filters=filters)
```
    return response['SecurityGroups']
```
</p>
</details>

5. Below the code window for Lambda function handler and role, select the execution role you created earlier.
6. Under Advanced settings, increase the Timeout to 5 seconds.  If you are updating several security groups with this function, you might have to increase the timeout by even more time. Finally, click Next.
7. After confirming your settings are correct, click Create function.



## Test Lambda Function
Now that you have created your function, it’s time to test it and initialize your security group:

1.  In the Lambda console on the Functions page, choose your function, choose the Actions drop-down menu, and then Configure test event.
2.  Enter the following as your sample event, which will represent an SNS notification.

```
{
  "Records": [
    {
      "EventVersion": "1.0",
      "EventSubscriptionArn": "arn:aws:sns:EXAMPLE",
      "EventSource": "aws:sns",
      "Sns": {
        "SignatureVersion": "1",
        "Timestamp": "1970-01-01T00:00:00.000Z",
        "Signature": "EXAMPLE",
        "SigningCertUrl": "EXAMPLE",
        "MessageId": "95df01b4-ee98-5cb9-9903-4c221d41eb5e",
        "Message": "{\"create-time\": \"yyyy-mm-ddThh:mm:ss+00:00\", \"synctoken\": \"0123456789\", \"md5\": \"7fd59f5c7f5cf643036cbd4443ad3e4b\", \"url\": \"https://ip-ranges.amazonaws.com/ip-ranges.json\"}",
        "Type": "Notification",
        "UnsubscribeUrl": "EXAMPLE",
        "TopicArn": "arn:aws:sns:EXAMPLE",
        "Subject": "TestInvoke"
      }
    }
  ]
}
```
3.  After you’ve added the test event, click Save and test. Your Lambda function will be invoked, and you should see log output at the bottom of the console similar to the following.
<pre>
Updating from https://ip-ranges.amazonaws.com/ip-ranges.json
MD5 Mismatch: got <b>2e967e943cf98ae998efeec05d4f351c</b> expected 7fd59f5c7f5cf643036cbd4443ad3e4b: Exception
Traceback (most recent call last):
  File "/var/task/lambda_function.py", line 29, in lambda_handler
    ip_ranges = json.loads(get_ip_groups_json(message['url'], message['md5']))
  File "/var/task/lambda_function.py", line 50, in get_ip_groups_json
    raise Exception('MD5 Missmatch: got ' + hash + ' expected ' + expected_hash)
Exception: MD5 Mismatch: got <b>2e967e943cf98ae998efeec05d4f351c</b> expected 7fd59f5c7f5cf643036cbd4443ad3e4b
</pre>
You will see a message indicating there was a hash mismatch. Normally, a real SNS notification from the IP Ranges SNS topic will include the right hash, but because our sample event is a test case representing the event, you will need to update the sample event manually to have the expected hash.

4.  Edit the sample event again, and this time change the md5 hash **that is bold** to be the first hash provided in the log output. In this example, we would update the sample event with the hash “2e967e943cf98ae998efeec05d4f351c”.


5.  Click Save and test, and your Lambda function will be invoked.

This time, you should see output indicating your security group was properly updated. If you go back to the EC2 console and view the security group you created, you will now see all the CloudFront IP ranges added as allowed points of ingress. If your log output is different, it should help you identify the issue.

## Configure your Lambda function’s trigger
After you have validated that your function is executing properly, it’s time to connect it to the SNS topic for IP changes. To do this, use the AWS Command Line Interface (CLI). Enter the following command, making sure to replace <Lambda ARN> with the Amazon Resource Name (ARN) of your Lambda function. You will find this ARN at the top right when viewing the configuration of your Lambda function.

`aws sns subscribe --topic-arn arn:aws:sns:us-east-1:806199016981:AmazonIpSpaceChanged --protocol lambda --notification-endpoint <Lambda ARN>`

You should receive an ARN of your Lambda function’s SNS subscription. Your Lambda function will now be invoked whenever AWS publishes new IP ranges!
***

Copyright 2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License"). You may not use this file except in compliance with the License. A copy of the License is located at

    http://aws.amazon.com/apache2.0/

or in the "license" file accompanying this file. This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

```
aws sns subscribe --topic-arn arn:aws:sns:us-east-1:806199016981:AmazonIpSpaceChanged --protocol lambda --notification-endpoint <Lambda ARN>
```
## NOTE: Take a look at the groups you created and they're now populated with a whole bunch of entries for CloudFront IP's.

AWS Console -> Elastic Beanstalk -> Application -> Environment -> Configuration -> Instance - Update the groups to include the group-id of the group you created.
The Lambda Function will keep your IP list in sync.



[Full article](https://aws.amazon.com/blogs/security/how-to-automatically-update-your-security-groups-for-amazon-cloudfront-and-aws-waf-by-using-aws-lambda/)


## What's next?

Here are some useful links to expand your knowledge and improve AWS services security on your project:
* How to secure AWS like a boss [article](https://www.infoworld.com/article/3026395/security/how-to-secure-amazon-web-services-like-a-boss.html)
* Top 7 AWS security [issues](https://www.threatstack.com/blog/what-you-need-to-know-about-the-top-7-aws-security-issues)
* AWS cloud security [guide](https://aws.amazon.com/security/)
