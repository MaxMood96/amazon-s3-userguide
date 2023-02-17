# How Amazon S3 authorizes a request<a name="how-s3-evaluates-access-control"></a>

When Amazon S3 receives a request—for example, a bucket or an object operation—it first verifies that the requester has the necessary permissions\. Amazon S3 evaluates all the relevant access policies, user policies, and resource\-based policies \(bucket policy, bucket ACL, object ACL\) in deciding whether to authorize the request\. 

In order to determine whether the requester has permission to perform the specific operation, Amazon S3 does the following, in order, when it receives a request:

1. Converts all the relevant access policies \(user policy, bucket policy, ACLs\) at run time into a set of policies for evaluation\.

1. Evaluates the resulting set of policies in the following steps\. In each step, Amazon S3 evaluates a subset of policies in a specific context, based on the context authority\. 

   1. **User context** – In the user context, the parent account to which the user belongs is the context authority\.

      Amazon S3 evaluates a subset of policies owned by the parent account\. This subset includes the user policy that the parent attaches to the user\. If the parent also owns the resource in the request \(bucket, object\), Amazon S3 also evaluates the corresponding resource policies \(bucket policy, bucket ACL, and object ACL\) at the same time\. 

      A user must have permission from the parent account to perform the operation\.

      This step applies only if the request is made by a user in an AWS account\. If the request is made by using the root user credentials of an AWS account, Amazon S3 skips this step\.

   1. **Bucket context** – In the bucket context, Amazon S3 evaluates policies owned by the AWS account that owns the bucket\. 

      If the request is for a bucket operation, the requester must have permission from the bucket owner\. If the request is for an object, Amazon S3 evaluates all the policies owned by the bucket owner to check if the bucket owner has not explicitly denied access to the object\. If there is an explicit deny set, Amazon S3 does not authorize the request\. 

   1. **Object context** – If the request is for an object, Amazon S3 evaluates the subset of policies owned by the object owner\. 

Following are some of the example scenarios that illustrate how Amazon S3 authorizes a request\.

**Example Requester is an IAM principal**  
If the requester is an IAM principal, Amazon S3 must determine if the parent AWS account to which the principal belongs has granted the principal necessary permission to perform the operation\. In addition, if the request is for a bucket operation, such as a request to list the bucket content, Amazon S3 must verify that the bucket owner has granted permission for the requester to perform the operation\. To perform a specific operation on a resource, an IAM principal needs permission from both the parent AWS account to which it belongs and the AWS account that owns the resource\.

 

**Example Requester is an IAM principal – If the request is for an operation on an object that the bucket owner doesn't own\.**  
If the request is for an operation on an object that the bucket owner does not own, in addition to making sure the requester has permissions from the object owner, Amazon S3 must also check the bucket policy to ensure the bucket owner has not set explicit deny on the object\. A bucket owner \(who pays the bill\) can explicitly deny access to objects in the bucket regardless of who owns it\. The bucket owner can also delete any object in the bucket\.  
By default, when another AWS account uploads an object to your S3 bucket, that account \(the object writer\) owns the object, has access to it, and can grant other users access to it through access control lists \(ACLs\)\. You can use Object Ownership to change this default behavior so that ACLs are disabled and you, as the bucket owner, automatically own every object in your bucket\. As a result, access control for your data is based on policies, such as IAM policies, S3 bucket policies, virtual private cloud \(VPC\) endpoint policies, and AWS Organizations service control policies \(SCPs\)\. For more information, see [Controlling ownership of objects and disabling ACLs for your bucket](about-object-ownership.md)\.

For more information about how Amazon S3 evaluates access policies to authorize or deny requests for bucket operations and object operations, see the following topics:

**Topics**
+ [How Amazon S3 authorizes a request for a bucket operation](access-control-auth-workflow-bucket-operation.md)
+ [How Amazon S3 authorizes a request for an object operation](access-control-auth-workflow-object-operation.md)