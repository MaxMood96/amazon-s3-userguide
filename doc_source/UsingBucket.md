# Buckets overview<a name="UsingBucket"></a>

To upload your data \(photos, videos, documents, etc\.\) to Amazon S3, you must first create an S3 bucket in one of the AWS Regions\. 

A bucket is a container for objects stored in Amazon S3\. You can store any number of objects in a bucket and can have up to 100 buckets in your account\. To request an increase, visit the [Service Quotas console](https://console.aws.amazon.com/servicequotas/home/services/s3/quotas/)\.

Every object is contained in a bucket\. For example, if the object named `photos/puppy.jpg` is stored in the `DOC-EXAMPLE-BUCKET` bucket in the US West \(Oregon\) Region, then it is addressable by using the URL `https://DOC-EXAMPLE-BUCKET.s3.us-west-2.amazonaws.com/photos/puppy.jpg`\. For more information, see [Accessing a Bucket](access-bucket-intro.md)\. 

In terms of implementation, buckets and objects are AWS resources, and Amazon S3 provides APIs for you to manage them\. For example, you can create a bucket and upload objects using the Amazon S3 API\. You can also use the Amazon S3 console to perform these operations\. The console uses the Amazon S3 APIs to send requests to Amazon S3\. 

This section describes how to work with buckets\. For information about working with objects, see [Amazon S3 objects overview](UsingObjects.md)\.

 Amazon S3 supports global buckets, which means that each bucket name must be unique across all AWS accounts in all the AWS Regions within a partition\. A partition is a grouping of Regions\. AWS currently has three partitions: `aws` \(Standard Regions\), `aws-cn` \(China Regions\), and `aws-us-gov` \(AWS GovCloud \(US\)\)\. 

After a bucket is created, the name of that bucket cannot be used by another AWS account in the same partition until the bucket is deleted\. You should not depend on specific bucket naming conventions for availability or security verification purposes\. For bucket naming guidelines, see [Bucket naming rules](bucketnamingrules.md)\.

Amazon S3 creates buckets in a Region that you specify\. To reduce latency, minimize costs, or address regulatory requirements, choose any AWS Region that is geographically close to you\. For example, if you reside in Europe, you might find it advantageous to create buckets in the Europe \(Ireland\) or Europe \(Frankfurt\) Regions\. For a list of Amazon S3 Regions, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/s3.html) in the *AWS General Reference*\.

**Note**  
Objects that belong to a bucket that you create in a specific AWS Region never leave that Region, unless you explicitly transfer them to another Region\. For example, objects that are stored in the Europe \(Ireland\) Region never leave it\. 

**Topics**
+ [About permissions](#about-access-permissions-create-bucket)
+ [Managing public access to buckets](#block-public-access-intro)
+ [Bucket configuration options](#bucket-config-options-intro)

## About permissions<a name="about-access-permissions-create-bucket"></a>

You can use your AWS account root user credentials to create a bucket and perform any other Amazon S3 operation\. However, we recommend that you do not use the root user credentials of your AWS account to make requests, such as to create a bucket\. Instead, create an AWS Identity and Access Management \(IAM\) user, and grant that user full access \(users by default have no permissions\)\. 

These users are referred to as *administrators*\. You can use the administrator user credentials, instead of the root user credentials of your account, to interact with AWS and perform tasks, such as create a bucket, create users, and grant them permissions\. 

For more information, see [AWS account root user credentials and IAM user credentials](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html) in the *AWS General Reference* and [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

The AWS account that creates a resource owns that resource\. For example, if you create an IAM user in your AWS account and grant the user permission to create a bucket, the user can create a bucket\. But the user does not own the bucket; the AWS account that the user belongs to owns the bucket\. The user needs additional permission from the resource owner to perform any other bucket operations\. For more information about managing permissions for your Amazon S3 resources, see [Identity and access management in Amazon S3](s3-access-control.md)\.

## Managing public access to buckets<a name="block-public-access-intro"></a>

Public access is granted to buckets and objects through bucket policies, access control lists \(ACLs\), or both\. To help you manage public access to Amazon S3 resources, Amazon S3 provides settings to block public access\. Amazon S3 Block Public Access settings can override ACLs and bucket policies so that you can enforce uniform limits on public access to these resources\. You can apply Block Public Access settings to individual buckets or to all buckets in your account\.

To ensure that all of your Amazon S3 buckets and objects have their public access blocked, all four settings for Block Public Access are enabled by default when you create a new bucket\. We recommend that you turn on all four settings for Block Public Access for your account too\. These settings block all public access for all current and future buckets\.

Before applying these settings, verify that your applications will work correctly without public access\. If you require some level of public access to your buckets or objects—for example, to host a static website, as described at [Hosting a static website using Amazon S3](WebsiteHosting.md)—you can customize the individual settings to suit your storage use cases\. For more information, see [Blocking public access to your Amazon S3 storage](access-control-block-public-access.md)\.

 However, we highly recommend keeping Block Public Access enabled\. If you want to keep all four Block Public Access settings enabled and host a static website, you can use Amazon CloudFront origin access control \(OAC\)\. Amazon CloudFront provides the capabilities required to set up a secure static website\. Amazon S3 static websites support only HTTP endpoints\. Amazon CloudFront uses the durable storage of Amazon S3 while providing additional security headers, such as HTTPS\. HTTPS adds security by encrypting a normal HTTP request and protecting against common cyberattacks\. 

For more information, see [Getting started with a secure static website](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/getting-started-secure-static-website-cloudformation-template.html) in the *Amazon CloudFront Developer Guide*\.

**Note**  
If you see an `Error` when you list your buckets and their public access settings, you might not have the required permissions\. Make sure that you have the following permissions added to your user or role policy:  

```
s3:GetAccountPublicAccessBlock
s3:GetBucketPublicAccessBlock
s3:GetBucketPolicyStatus
s3:GetBucketLocation
s3:GetBucketAcl
s3:ListAccessPoints
s3:ListAllMyBuckets
```
In some rare cases, requests can also fail because of an AWS Region outage\.

## Bucket configuration options<a name="bucket-config-options-intro"></a>

Amazon S3 supports various options for you to configure your bucket\. For example, you can configure your bucket for website hosting, add a configuration to manage the lifecycle of objects in the bucket, and configure the bucket to log all access to the bucket\. Amazon S3 supports subresources for you to store and manage the bucket configuration information\. You can use the Amazon S3 API to create and manage these subresources\. However, you can also use the console or the AWS SDKs\. 

**Note**  
There are also object\-level configurations\. For example, you can configure object\-level permissions by configuring an access control list \(ACL\) specific to that object\.

These are referred to as subresources because they exist in the context of a specific bucket or object\. The following table lists subresources that enable you to manage bucket\-specific configurations\. 


| Subresource | Description | 
| --- | --- | 
|   *cors* \(cross\-origin resource sharing\)   |   You can configure your bucket to allow cross\-origin requests\. For more information, see [Using cross\-origin resource sharing \(CORS\)](cors.md)\.  | 
|   *event notification*   |  You can enable your bucket to send you notifications of specified bucket events\.  For more information, see [Amazon S3 Event Notifications](NotificationHowTo.md)\.  | 
| lifecycle |  You can define lifecycle rules for objects in your bucket that have a well\-defined lifecycle\. For example, you can define a rule to archive objects one year after creation, or delete an object 10 years after creation\.  For more information, see [Managing your storage lifecycle](object-lifecycle-mgmt.md)\.   | 
|   *location*   |   When you create a bucket, you specify the AWS Region where you want Amazon S3 to create the bucket\. Amazon S3 stores this information in the location subresource and provides an API for you to retrieve this information\.   | 
|   *logging*   |  Logging enables you to track requests for access to your bucket\. Each access log record provides details about a single access request, such as the requester, bucket name, request time, request action, response status, and error code, if any\. Access log information can be useful in security and access audits\. It can also help you learn about your customer base and understand your Amazon S3 bill\.   For more information, see [Logging requests using server access logging](ServerLogs.md)\.   | 
|   *object locking*   |  To use S3 Object Lock, you must enable it for a bucket\. You can also optionally configure a default retention mode and period that applies to new objects that are placed in the bucket\.  For more information, see [Bucket configuration](object-lock-overview.md#object-lock-bucket-config)\.   | 
|   *policy* and *ACL* \(access control list\)   |  All your resources \(such as buckets and objects\) are private by default\. Amazon S3 supports both bucket policy and access control list \(ACL\) options for you to grant and manage bucket\-level permissions\. Amazon S3 stores the permission information in the *policy* and *acl* subresources\. For more information, see [Identity and access management in Amazon S3](s3-access-control.md)\.  | 
|   *replication*   |  Replication is the automatic, asynchronous copying of objects across buckets in different or the same AWS Regions\. For more information, see [Replicating objects](replication.md)\.  | 
|   *requestPayment*   |  By default, the AWS account that creates the bucket \(the bucket owner\) pays for downloads from the bucket\. Using this subresource, the bucket owner can specify that the person requesting the download will be charged for the download\. Amazon S3 provides an API for you to manage this subresource\. For more information, see [Using Requester Pays buckets for storage transfers and usage](RequesterPaysBuckets.md)\.  | 
|   *tagging*   |  You can add cost allocation tags to your bucket to categorize and track your AWS costs\. Amazon S3 provides the *tagging* subresource to store and manage tags on a bucket\. Using tags you apply to your bucket, AWS generates a cost allocation report with usage and costs aggregated by your tags\.  For more information, see [Billing and usage reporting for S3 buckets](BucketBilling.md)\.   | 
|   *transfer acceleration*   |  Transfer Acceleration enables fast, easy, and secure transfers of files over long distances between your client and an S3 bucket\. Transfer Acceleration takes advantage of the globally distributed edge locations of Amazon CloudFront\. For more information, see [Configuring fast, secure file transfers using Amazon S3 Transfer Acceleration](transfer-acceleration.md)\.  | 
| versioning |  Versioning helps you recover accidental overwrites and deletes\.  We recommend versioning as a best practice to recover objects from being deleted or overwritten by mistake\.  For more information, see [Using versioning in S3 buckets](Versioning.md)\.  | 
|  website |  You can configure your bucket for static website hosting\. Amazon S3 stores this configuration by creating a *website* subresource\. For more information, see [Hosting a static website using Amazon S3](WebsiteHosting.md)\.   | 