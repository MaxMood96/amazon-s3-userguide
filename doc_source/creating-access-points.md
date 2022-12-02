# Creating access points<a name="creating-access-points"></a>

Amazon S3 provides functionality for creating and managing access points\. You can create S3 access points by using the AWS Management Console, AWS Command Line Interface \(AWS CLI\), AWS SDKs, or Amazon S3 REST API\. 

By default, you can create up to 10,000 access points per Region for each of your AWS accounts\. If you need more than 10,000 access points for a single account in a single Region, you can request a service quota increase\. For more information about service quotas and requesting an increase, see [AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *AWS General Reference*\.

**Note**  
Because you might want to publicize your access point name so that other users can use the access point, avoid including sensitive information in the access point name\. Access point names are published in a publicly accessible database known as the Domain Name System \(DNS\)\.

## Rules for naming Amazon S3 access points<a name="access-points-names"></a>

Access point names must meet the following conditions:
+ Must be unique within a single AWS account and Region
+ Must comply with DNS naming restrictions
+ Must begin with a number or lowercase letter
+ Must be between 3 and 50 characters long
+ Can't begin or end with a hyphen \(`-`\)
+ Can't contain underscores \(`_`\), uppercase letters, or periods \(`.`\)
+ Can't end with the suffix `-s3alias`\. This suffix is reserved for access point alias names\. For more information, see [Using a bucket\-style alias for your S3 bucket access point](access-points-alias.md)\.

To create an access point, see the following topics\.

**Topics**
+ [Rules for naming Amazon S3 access points](#access-points-names)
+ [Creating an access point](create-access-points.md)
+ [Creating access points restricted to a virtual private cloud](access-points-vpc.md)
+ [Managing public access to access points](access-points-bpa-settings.md)