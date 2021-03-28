---
title: "Three ways to log S3 Bucket Activities"
date: 2020-12-20
hero: /images/posts/aws/3-ways-to-log-s3/s3-logo.svg
menu:
    sidebar:
        name: S3-log
        identifier: aws-s3-log
        parent: aws
        weight: 10
---

A common requirement is to monitor S3 bucket activities, especially for the purpose of auditing. In this blog, I will introduce
three ways to implement this feature.

![Alt text](/images/posts/aws/s3-logo.png)

### 1. Use S3 properties
Enable Server Access Logging for an S3 Bucket. It is one of the S3 bucket properties. You just need to select a target bucket and prefix to complete the setup. 
The details are available here: https://docs.aws.amazon.com/AmazonS3/latest/user-guide/server-access-logging.html
* Pros: 
    * less infrastructure
    * no code involved
    * easy to search log for a specific bucket.
* Cons: 
    * the target bucket must be in public ip which is a deal breaker sometimes. 
    * no UI.

### 2. Use CloudTrail
Use CloudTrail to create a trail to monitor S3 bucket.
* Pros:
    * less infrastructure 
    * no code involved
    * UI for browsing 
    * works for both private and public ip target bucket.
* Cons: 
    * when monitoring multiple buckets, it is not easy to search a specific bucket log since there is no prefix setup 
for each source bucket.

### 3. Use Lambda
Use Lambda to capture S3 bucket events.
* Pros:
    * easy customized search
    * works for both private and public for target bucket
* Cons: 
    * more infrastructure 
    * code involved 
    * no UI.

I used method 3 in my application. Since method 1 requires public ip for target bucket which breaks security policy. 
Method 2 is a perfect solution if we only need to monitor one S3 bucket. For monitoring multiple buckets, 
it is really not easy to search activities for a specific bucket log. Method 3 works well for monitor multiple S3 buckets, 
but like I mentioned before it needs to maintain infrastructure, although the code is simple.
