# Developing with Amazon S3 using the AWS CLI<a name="setup-aws-cli"></a>

Follow these steps to download and configure AWS Command Line Interface \(AWS CLI\)\. 

For a list of Amazon S3 AWS CLI commands, see the following pages in the *AWS CLI Command Reference*:
+ [s3](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/index.html)
+ [s3api](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3api/index.html)
+ [s3control](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3control/index.html)

**Note**  
Services in AWS, such as Amazon S3, require that you provide credentials when you access them\. The service can then determine whether you have permissions to access the resources that it owns\. The console requires your password\. You can create access keys for your AWS account to access the AWS CLI or API\. However, we don't recommend that you access AWS using the credentials for your AWS account\. Instead, we recommend that you use AWS Identity and Access Management \(IAM\)\. Create an IAM user, add the user to an IAM group with administrative permissions, and then grant administrative permissions to the IAM user that you created\. You can then access AWS using a special URL and the credentials of that IAM user\. For instructions, go to [Creating Your First IAM user and Administrators Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.

**To set up the AWS CLI**

1.  Download and configure the AWS CLI\. For instructions, see the following topics in the *AWS Command Line Interface User Guide*: 
   +  [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) 
   +  [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) 

1.  Add a named profile for the administrator user in the AWS CLI config file\. You use this profile when executing the AWS CLI commands\. For more information, see [Named profiles for the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)in the *AWS Command Line Interface User Guide*\.

   ```
   [adminuser] 
   aws_access_key_id = adminuser access key ID 
   aws_secret_access_key = adminuser secret access key 
   region = aws-region
   ```

    For a list of available AWS Regions, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *AWS General Reference*\. 

1.  Verify the setup by typing the following commands at the command prompt\. 
   + Try the `help` command to verify that the AWS CLI is installed on your computer: 

     ```
     aws help  
     ```
   + Run an `S3` command using the `adminuser` credentials that you just created\. To do this, add the `--profile` parameter to your command to specify the profile name\. In this example, the `ls` command lists buckets in your account\. The AWS CLI uses the `adminuser` credentials to authenticate the request\. 

     ```
      aws s3 ls --profile adminuser
     ```