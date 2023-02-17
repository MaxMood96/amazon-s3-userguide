# Configuring MFA delete<a name="MultiFactorAuthenticationDelete"></a>

When working with S3 Versioning in Amazon S3 buckets, you can optionally add another layer of security by configuring a bucket to enable *MFA \(multi\-factor authentication\) delete*\. When you do this, the bucket owner must include two forms of authentication in any request to delete a version or change the versioning state of the bucket\.

MFA delete requires additional authentication for either of the following operations:
+ Changing the versioning state of your bucket
+ Permanently deleting an object version

MFA delete requires two forms of authentication together:
+ Your security credentials
+ The concatenation of a valid serial number, a space, and the six\-digit code displayed on an approved authentication device

MFA delete thus provides added security if, for example, your security credentials are compromised\. MFA delete can help prevent accidental bucket deletions by requiring the user who initiates the delete action to prove physical possession of an MFA device with an MFA code and adding an extra layer of friction and security to the delete action\.

To identify buckets that have MFA delete enabled, you can use Amazon S3 Storage Lens metrics\. S3 Storage Lens is a cloud\-storage analytics feature that you can use to gain organization\-wide visibility into object\-storage usage and activity\. For more information, see [ Assessing your storage activity and usage with S3 Storage Lens](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage_lens?icmpid=docs_s3_user_guide_MultiFactorAuthenticationDelete.html)\. For a complete list of metrics, see [ S3 Storage Lens metrics glossary](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage_lens_metrics_glossary.html?icmpid=docs_s3_user_guide_MultiFactorAuthenticationDelete.html)\.

The bucket owner, the AWS account that created the bucket \(root account\), and all authorized users can enable versioning\. However, only the bucket owner \(root account\) can enable MFA delete\. For more information, see [Securing Access to AWS Using MFA](http://aws.amazon.com/blogs/security/securing-access-to-aws-using-mfa-part-3/) on the AWS Security Blog\.

**Note**  
To use MFA delete with versioning, you enable `MFA Delete`\. However, you cannot enable `MFA Delete` using the AWS Management Console\. You must use the AWS Command Line Interface \(AWS CLI\) or the API\.   
For examples of using MFA delete with versioning, see the examples section in the topic [Enabling versioning on buckets](manage-versioning-examples.md)\.  
You cannot use MFA delete with lifecycle configurations\. For more information about lifecycle configurations and how they interact with other configurations, see [Lifecycle and other bucket configurations](lifecycle-and-other-bucket-config.md)\.

To enable or disable MFA delete, you use the same API that you use to configure versioning on a bucket\. Amazon S3 stores the MFA delete configuration in the same *versioning* subresource that stores the bucket's versioning status\.

```
<VersioningConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/"> 
  <Status>VersioningState</Status>
  <MfaDelete>MfaDeleteState</MfaDelete>  
</VersioningConfiguration>
```

To use MFA delete, you can use either a hardware or virtual MFA device to generate an authentication code\. The following example shows a generated authentication code displayed on a hardware device\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/userguide/images/MFADevice.png)

MFA delete and MFA\-protected API access are features intended to provide protection for different scenarios\. You configure MFA delete on a bucket to help ensure that the data in your bucket cannot be accidentally deleted\. MFA\-protected API access is used to enforce another authentication factor \(MFA code\) when accessing sensitive Amazon S3 resources\. You can require any operations against these Amazon S3 resources to be done with temporary credentials created using MFA\. For an example, see [Requiring MFA](example-bucket-policies.md#example-bucket-policies-MFA)\. 

For more information about how to purchase and activate an authentication device, see [Multi\-factor authentication](http://aws.amazon.com/iam/details/mfa/)\.

## To enable S3 Versioning and configure MFA delete<a name="enable-versioning-mfa-delete"></a>

### Using the AWS CLI<a name="enable-versioning-mfa-delete-cli"></a>

The following example enables S3 Versioning and multi\-factor authentication \(MFA\) delete on a bucket\.

```
aws s3api put-bucket-versioning --bucket DOC-EXAMPLE-BUCKET1 --versioning-configuration Status=Enabled,MFADelete=Enabled --mfa "SERIAL 123456"
```

### Using the REST API<a name="enable-versioning-mfa-delete-rest-api"></a>

For more information about specifying MFA delete using the Amazon S3 REST API, see [PutBucketVersioning](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketVersioning.html) *Amazon Simple Storage Service API Reference*\.