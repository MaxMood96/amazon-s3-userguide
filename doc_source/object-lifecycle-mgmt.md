# Managing your storage lifecycle<a name="object-lifecycle-mgmt"></a>

To manage your objects so that they are stored cost effectively throughout their lifecycle, configure their *Amazon S3 Lifecycle*\. An *S3 Lifecycle configuration* is a set of rules that define actions that Amazon S3 applies to a group of objects\. There are two types of actions:
+ **Transition actions** – These actions define when objects transition to another storage class\. For example, you might choose to transition objects to the S3 Standard\-IA storage class 30 days after creating them, or archive objects to the S3 Glacier Flexible Retrieval storage class one year after creating them\. For more information, see [Using Amazon S3 storage classes](storage-class-intro.md)\. 

  There are costs associated with lifecycle transition requests\. For pricing information, see [Amazon S3 pricing](https://aws.amazon.com/s3/pricing/)\.
+ **Expiration actions** – These actions define when objects expire\. Amazon S3 deletes expired objects on your behalf\. 

  Lifecycle expiration costs depend on when you choose to expire objects\. For more information, see [Expiring objects](lifecycle-expire-general-considerations.md)\.

If there is any delay between when an object becomes eligible for a lifecycle action and when Amazon S3 transfers or expires your object, billing changes are applied as soon as the object becomes eligible for the lifecycle action\. For example, if an object is scheduled to expire and Amazon S3 does not immediately expire the object, you won't be charged for storage after the expiration time\. The one exception to this behavior is if you have a lifecycle rule to transition to the S3 Intelligent\-Tiering storage class\. In that case, billing changes do not occur until the object has transitioned to S3 Intelligent\-Tiering\. 

For more information about S3 Lifecycle rules, see [Lifecycle configuration elements](intro-lifecycle-rules.md)\. 

To get detailed metrics for S3 Lifecycle, you can use Amazon S3 Storage Lens metrics\. S3 Storage Lens is a cloud\-storage analytics feature that you can use to gain organization\-wide visibility into object\-storage usage and activity\. S3 Storage Lens provides S3 Lifecycle rule\-count metrics and metrics that you can use to identify buckets with S3 Versioning enabled or a high percentage of noncurrent version bytes\. For more information, see [ Using S3 Storage Lens to optimize your storage costs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-lens-optimize-storage.html?icmpid=docs_s3_user_guide_object-lifecycle-mgmt.html)\.

## Managing object lifecycle<a name="lifecycle-config-overview-what"></a>

Define S3 Lifecycle configuration rules for objects that have a well\-defined lifecycle\. For example: 
+ If you upload periodic logs to a bucket, your application might need them for a week or a month\. After that, you might want to delete them\.
+ Some documents are frequently accessed for a limited period of time\. After that, they are infrequently accessed\. At some point, you might not need real\-time access to them, but your organization or regulations might require you to archive them for a specific period\. After that, you can delete them\. 
+ You might upload some types of data to Amazon S3 primarily for archival purposes\. For example, you might archive digital media, financial and healthcare records, raw genomics sequence data, long\-term database backups, and data that must be retained for regulatory compliance\.

With S3 Lifecycle configuration rules, you can tell Amazon S3 to transition objects to less\-expensive storage classes, or archive or delete them\.

## Creating a lifecycle configuration<a name="lifecycle-config-overview-how"></a>

An S3 Lifecycle configuration is an XML file that consists of a set of rules with predefined actions that you want Amazon S3 to perform on objects during their lifetime\. 

You can also configure the lifecycle by using the Amazon S3 console, REST API, AWS SDKs, and the AWS Command Line Interface \(AWS CLI\)\. For more information, see [Setting lifecycle configuration on a bucket](how-to-set-lifecycle-configuration-intro.md)\.

Amazon S3 provides a set of REST API operations for managing lifecycle configuration on a bucket\. Amazon S3 stores the configuration as a *lifecycle subresource* that is attached to your bucket\. For details, see the following:

[PUT Bucket lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUTlifecycle.html)

[GET Bucket lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketGETlifecycle.html)

[DELETE Bucket lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETElifecycle.html)

For more information about creating a lifecycle configuration, see the following topics:

**Topics**
+ [Managing object lifecycle](#lifecycle-config-overview-what)
+ [Creating a lifecycle configuration](#lifecycle-config-overview-how)
+ [Transitioning objects using Amazon S3 Lifecycle](lifecycle-transition-general-considerations.md)
+ [Expiring objects](lifecycle-expire-general-considerations.md)
+ [Setting lifecycle configuration on a bucket](how-to-set-lifecycle-configuration-intro.md)
+ [Lifecycle and other bucket configurations](lifecycle-and-other-bucket-config.md)
+ [Configuring Lifecycle event notifications](lifecycle-configure-notification.md)
+ [Lifecycle configuration elements](intro-lifecycle-rules.md)
+ [Examples of S3 Lifecycle configuration](lifecycle-configuration-examples.md)