# Configuring an S3 Bucket Key at the object level<a name="configuring-bucket-key-object"></a>

When you perform a PUT or COPY operation using the REST API, AWS SDKs, or AWS CLI, you can enable or disable an S3 Bucket Key at the object level by adding the `x-amz-server-side-encryption-bucket-key-enabled` request header with a `true` or `false` value\. S3 Bucket Keys reduce the cost of server\-side encryption using AWS Key Management Service \(AWS KMS\) \(SSE\-KMS\) by decreasing request traffic from Amazon S3 to AWS KMS\. For more information, see [Reducing the cost of SSE\-KMS with Amazon S3 Bucket Keys](bucket-key.md)\. 

When you configure an S3 Bucket Key for an object using a PUT or COPY operation, Amazon S3 only updates the settings for that object\. The S3 Bucket Key settings for the destination bucket do not change\. If you submit a PUT or COPY request for a KMS\-encrypted object into a bucket with S3 Bucket Keys enabled, your object level operation will automatically use S3 Bucket Keys unless you disable the keys in the request header\. If you don't specify an S3 Bucket Key for your object, Amazon S3 applies the S3 Bucket Key settings for the destination bucket to the object\.

**Prerequisite:**  
Before you configure your object to use an S3 Bucket Key, review  [Changes to note before enabling an S3 Bucket Key](bucket-key.md#bucket-key-changes)\. 

**Topics**
+ [Amazon S3 Batch Operations](#bucket-key-object-bops)
+ [Using the REST API](#bucket-key-object-rest)
+ [Using the AWS SDK for Java \(PutObject\)](#bucket-key-object-sdk)
+ [Using the AWS CLI \(PutObject\)](#bucket-key-object-cli)

## Amazon S3 Batch Operations<a name="bucket-key-object-bops"></a>

To encrypt your existing Amazon S3 objects, you can use Amazon S3 Batch Operations\. You provide S3 Batch Operations with a list of objects to operate on, and Batch Operations calls the respective API to perform the specified operation\. 

You can use the [S3 Batch Operations Copy operation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/batch-ops-copy-object.html) to copy existing unencrypted objects and write them back to the same bucket as encrypted objects\. A single Batch Operations job can perform the specified operation on billions of objects\. For more information, see [Performing large\-scale batch operations on Amazon S3 objects](batch-ops.md) and [Encrypting objects with Amazon S3 Batch Operations](http://aws.amazon.com/blogs/storage/encrypting-objects-with-amazon-s3-batch-operations/)\.

## Using the REST API<a name="bucket-key-object-rest"></a>

When you use SSE\-KMS, you can enable an S3 Bucket Key for an object by using the following API operations: 
+ [PutObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObject.html) – When you upload an object, you can specify the `x-amz-server-side-encryption-bucket-key-enabled` request header to enable or disable an S3 Bucket Key at the object level\. 
+ [CopyObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CopyObject.html) – When you copy an object and configure SSE\-KMS, you can specify the `x-amz-server-side-encryption-bucket-key-enabled` request header to enable or disable an S3 Bucket Key for your object\. 
+ [POST Object](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectPOST.html) – When you use a `POST` operation to upload an object and configure SSE\-KMS, you can use the `x-amz-server-side-encryption-bucket-key-enabled` form field to enable or disable an S3 Bucket Key for your object\.
+ [CreateMultipartUpload](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CreateMultipartUpload.html) – When you upload large objects by using the `CreateMultipartUpload` API operation and configure SSE\-KMS, you can use the `x-amz-server-side-encryption-bucket-key-enabled` request header to enable or disable an S3 Bucket Key for your object\.

To enable an S3 Bucket Key at the object level, include the `x-amz-server-side-encryption-bucket-key-enabled` request header\. For more information about SSE\-KMS and the REST API, see [Using the REST API](specifying-kms-encryption.md#KMSUsingRESTAPI)\.

## Using the AWS SDK for Java \(PutObject\)<a name="bucket-key-object-sdk"></a>

You can use the following example to configure an S3 Bucket Key at the object level using the AWS SDK for Java\.

------
#### [ Java ]

```
AmazonS3 s3client = AmazonS3ClientBuilder.standard()
    .withRegion(Regions.DEFAULT_REGION)
    .build();

String bucketName = "DOC-EXAMPLE-BUCKET1";
String keyName = "key name for object";
String contents = "file contents";

PutObjectRequest putObjectRequest = new PutObjectRequest(bucketName, keyName, contents)
    .withBucketKeyEnabled(true);
    
s3client.putObject(putObjectRequest);
```

------

## Using the AWS CLI \(PutObject\)<a name="bucket-key-object-cli"></a>

You can use the following AWS CLI example to configure an S3 Bucket Key at the object level as part of a `PutObject` request\.

```
aws s3api put-object --bucket DOC-EXAMPLE-BUCKET --key object key name --server-side-encryption aws:kms --bucket-key-enabled --body filepath
```