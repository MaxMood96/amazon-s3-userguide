# How Amazon S3 authorizes a request for a bucket operation<a name="access-control-auth-workflow-bucket-operation"></a>

When Amazon S3 receives a request for a bucket operation, Amazon S3 converts all the relevant permissions into a set of policies to evaluate at run time\. Relevant permissions include resource\-based  permissions \(for example, bucket policies and bucket access control lists\) and user policies if the request is from an IAM principal\. Amazon S3 then evaluates the resulting set of policies in a series of steps according to a specific context—user context or bucket context\. 

1. **User context** – If the requester is an IAM principal, the principal must have permission from the parent AWS account to which it belongs\. In this step, Amazon S3 evaluates a subset of policies owned by the parent account \(also referred to as the context authority\)\. This subset of policies includes the user policy that the parent account attaches to the principal\. If the parent also owns the resource in the request \(in this case, the bucket\), Amazon S3 also evaluates the corresponding resource policies \(bucket policy and bucket ACL\) at the same time\. Whenever a request for a bucket operation is made, the server access logs record the canonical ID of the requester\. For more information, see [Logging requests using server access logging](ServerLogs.md)\.

1. **Bucket context** – The requester must have permissions from the bucket owner to perform a specific bucket operation\. In this step, Amazon S3 evaluates a subset of policies owned by the AWS account that owns the bucket\. 

   The bucket owner can grant permission by using a bucket policy or bucket ACL\. Note that, if the AWS account that owns the bucket is also the parent account of an IAM principal, then it can configure bucket permissions in a user policy\. 

 The following is a graphical illustration of the context\-based evaluation for bucket operation\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/userguide/images/AccessControlAuthorizationFlowBucketResource.png)

The following examples illustrate the evaluation logic\. 

## Example 1: Bucket operation requested by bucket owner<a name="example1-policy-eval-logic"></a>

 In this example, the bucket owner sends a request for a bucket operation by using the root credentials of the AWS account\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/userguide/images/example10-policy-eval-logic.png)

 Amazon S3 performs the context evaluation as follows:

1.  Because the request is made by using the root user credentials of an AWS account, the user context is not evaluated\.

1.  In the bucket context, Amazon S3 reviews the bucket policy to determine if the requester has permission to perform the operation\. Amazon S3 authorizes the request\. 

## Example 2: Bucket operation requested by an AWS account that is not the bucket owner<a name="example2-policy-eval-logic"></a>

In this example, a request is made by using the root user credentials of AWS account 1111\-1111\-1111 for a bucket operation owned by AWS account 2222\-2222\-2222\. No IAM users are involved in this request\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/userguide/images/example20-policy-eval-logic.png)

In this case, Amazon S3 evaluates the context as follows:

1.  Because the request is made by using the root user credentials of an AWS account, the user context is not evaluated\.

1. In the bucket context, Amazon S3 examines the bucket policy\. If the bucket owner \(AWS account 2222\-2222\-2222\) has not authorized AWS account 1111\-1111\-1111 to perform the requested operation, Amazon S3 denies the request\. Otherwise, Amazon S3 grants the request and performs the operation\.

## Example 3: Bucket operation requested by an IAM principal whose parent AWS account is also the bucket owner<a name="example3-policy-eval-logic"></a>

 In the example, the request is sent by Jill, an IAM user in AWS account 1111\-1111\-1111, which also owns the bucket\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/userguide/images/example30-policy-eval-logic.png)

 Amazon S3 performs the following context evaluation:

1.  Because the request is from an IAM principal, in the user context, Amazon S3 evaluates all policies that belong to the parent AWS account to determine if Jill has permission to perform the operation\. 

    In this example, parent AWS account 1111\-1111\-1111, to which the principal belongs, is also the bucket owner\. As a result, in addition to the user policy, Amazon S3 also evaluates the bucket policy and bucket ACL in the same context, because they belong to the same account\.

1. Because Amazon S3 evaluated the bucket policy and bucket ACL as part of the user context, it does not evaluate the bucket context\.

## Example 4: Bucket operation requested by an IAM principal whose parent AWS account is not the bucket owner<a name="example4-policy-eval-logic"></a>

In this example, the request is sent by Jill, an IAM user whose parent AWS account is 1111\-1111\-1111, but the bucket is owned by another AWS account, 2222\-2222\-2222\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonS3/latest/userguide/images/example40-policy-eval-logic.png)

 Jill will need permissions from both the parent AWS account and the bucket owner\. Amazon S3 evaluates the context as follows:

1. Because the request is from an IAM principal, Amazon S3 evaluates the user context by reviewing the policies authored by the account to verify that Jill has the necessary permissions\. If Jill has permission, then Amazon S3 moves on to evaluate the bucket context; if not, it denies the request\.

1.  In the bucket context, Amazon S3 verifies that bucket owner 2222\-2222\-2222 has granted Jill \(or her parent AWS account\) permission to perform the requested operation\. If she has that permission, Amazon S3 grants the request and performs the operation; otherwise, Amazon S3 denies the request\. 