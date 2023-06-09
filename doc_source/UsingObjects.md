# Amazon S3 objects overview<a name="UsingObjects"></a>

Amazon S3 is an object store that uses unique key\-values to store as many objects as you want\. You store these objects in one or more buckets, and each object can be up to 5 TB in size\. An object consists of the following:

Key  
The name that you assign to an object\. You use the object key to retrieve the object\. For more information, see [Working with object metadata](UsingMetadata.md)\.

Version ID  
 Within a bucket, a key and version ID uniquely identify an object\. The version ID is a string that Amazon S3 generates when you add an object to a bucket\. For more information, see [Using versioning in S3 buckets](Versioning.md)\.

Value  
 The content that you are storing\.  
An object value can be any sequence of bytes\. Objects can range in size from zero to 5 TB\. For more information, see [Uploading objects](upload-objects.md)\.

Metadata  
A set of name\-value pairs with which you can store information regarding the object\. You can assign metadata, referred to as user\-defined metadata, to your objects in Amazon S3\. Amazon S3 also assigns system\-metadata to these objects, which it uses for managing objects\. For more information, see [Working with object metadata](UsingMetadata.md)\.

Subresources  
Amazon S3 uses the subresource mechanism to store object\-specific additional information\. Because subresources are subordinates to objects, they are always associated with some other entity such as an object or a bucket\. For more information, see [Object subresources](#ObjectAndSubResource)\.

Access control information  
You can control access to the objects you store in Amazon S3\. Amazon S3 supports both the resource\-based access control, such as an access control list \(ACL\) and bucket policies, and user\-based access control\. For more information about access control, see the following:  
+ [Access control best practices](access-control-best-practices.md)
+ [Access policy guidelines](access-policy-alternatives-guidelines.md)
+ [Identity and access management in Amazon S3](s3-access-control.md)
+ [Configuring ACLs](managing-acls.md)
Your Amazon S3 resources \(for example, buckets and objects\) are private by default\. You must explicitly grant permission for others to access these resources\. For more information about sharing objects, see [Sharing objects with presigned URLs](ShareObjectPreSignedURL.md)\.

## Object subresources<a name="ObjectAndSubResource"></a>

Amazon S3 defines a set of subresources associated with buckets and objects\. Subresources are subordinates to objects\. This means that subresources don't exist on their own\. They are always associated with some other entity, such as an object or a bucket\. 

 The following table lists the subresources associated with Amazon S3 objects\.


| Subresource | Description | 
| --- | --- | 
| acl | Contains a list of grants identifying the grantees and the permissions granted\. When you create an object, the acl identifies the object owner as having full control over the object\. You can retrieve an object ACL or replace it with an updated list of grants\. Any update to an ACL requires you to replace the existing ACL\. For more information about ACLs, see [Access control list \(ACL\) overview](acl-overview.md)\. | 