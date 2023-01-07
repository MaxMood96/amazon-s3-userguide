# Speeding up your website with Amazon CloudFront<a name="website-hosting-cloudfront-walkthrough"></a>

You can use [Amazon CloudFront](http://aws.amazon.com/cloudfront) to improve the performance of your Amazon S3 website\. CloudFront makes your website files \(such as HTML, images, and video\) available from data centers around the world \(known as *edge locations*\)\. When a visitor requests a file from your website, CloudFront automatically redirects the request to a copy of the file at the nearest edge location\. This results in faster download times than if the visitor had requested the content from a data center that is located farther away\.

CloudFront caches content at edge locations for a period of time that you specify\. If a visitor requests content that has been cached for longer than the expiration date, CloudFront checks the origin server to see if a newer version of the content is available\. If a newer version is available, CloudFront copies the new version to the edge location\. Changes that you make to the original content are replicated to edge locations as visitors request the content\. 

**Automating set up with an AWS CloudFormation template**  
For more information about using an AWS CloudFormation template to configure a secure static website that creates a CloudFront distribution to serve your website, see [Getting started with a secure static website](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/getting-started-secure-static-website-cloudformation-template.html) in the *Amazon CloudFront Developer Guide*\.

**Topics**
+ [Step 1: Create a CloudFront distribution](#create-distribution)
+ [Step 2: Update the record sets for your domain and subdomain](#update-record-sets)
+ [\(Optional\) Step 3: Check the log files](#check-log-files)

## Step 1: Create a CloudFront distribution<a name="create-distribution"></a>

First, you create a CloudFront distribution\. This makes your website available from data centers around the world\.

**To create a distribution with an Amazon S3 origin**

1. Open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. Choose **Create Distribution**\.

1. On the **Create Distribution** page, in the **Origin Settings** section, for **Origin Domain Name**, enter the Amazon S3 website endpoint for your bucket—for example, **example\.com\.s3\-website\.us\-west\-1\.amazonaws\.com**\.

   CloudFront fills in the **Origin ID** for you\.

1. For **Default Cache Behavior Settings**, keep the values set to the defaults\. 

   With the default settings for **Viewer Protocol Policy**, you can use HTTPS for your static website\. For more information these configuration options, see [Values that You Specify When You Create or Update a Web Distribution](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/WorkingWithDownloadDistributions.html#DownloadDistValuesYouSpecify) in the *Amazon CloudFront Developer Guide*\.

1. For **Distribution Settings**, do the following:

   1. Leave **Price Class** set to **Use All Edge Locations \(Best Performance\)**\.

   1. Set **Alternate Domain Names \(CNAMEs\)** to the root domain and `www` subdomain\. In this tutorial, these are `example.com` and `www.example.com`\.  
**Important**  
Before you perform this step, note the [requirements for using alternate domain names](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/CNAMEs.html#alternate-domain-names-requirements), in particular the need for a valid SSL/TLS certificate\. 

   1. For **SSL Certificate**, choose **Custom SSL Certificate \(example\.com\)**, and choose the custom certificate that covers the domain and subdomain names\.

      For more information, see [SSL Certificate](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesSSLCertificate) in the *Amazon CloudFront Developer Guide*\.

   1. In **Default Root Object**, enter the name of your index document, for example, `index.html`\. 

      If the URL used to access the distribution doesn't contain a file name, the CloudFront distribution returns the index document\. The **Default Root Object** should exactly match the name of the index document for your static website\. For more information, see [Configuring an index document](IndexDocumentSupport.md)\.

   1. Set **Logging** to **On**\.
**Important**  
When you create or update a distribution and enable CloudFront logging, CloudFront updates the bucket access control list \(ACL\) to give the `awslogsdelivery` account `FULL_CONTROL` permissions to write logs to your bucket\. For more information, see [Permissions required to configure standard logging and to access your log files](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html#AccessLogsBucketAndFileOwnership) in the *Amazon CloudFront Developer Guide*\. If the bucket that stores the logs uses the bucket owner enforced setting for S3 Object Ownership to disable ACLs, CloudFront cannot write logs to the bucket\. For more information, see [Controlling ownership of objects and disabling ACLs for your bucket](about-object-ownership.md)\.

   1. For **Bucket for Logs**, choose the logging bucket that you created\.

      For more information about configuring a logging bucket, see [\(Optional\) Logging web traffic](LoggingWebsiteTraffic.md)\.

   1. If you want to store the logs that are generated by traffic to the CloudFront distribution in a folder, in **Log Prefix**, enter the folder name\.

   1. Keep all other settings at their default values\.

1. Choose **Create Distribution**\.

1. To see the status of the distribution, find the distribution in the console and check the **Status** column\. 

   A status of `InProgress` indicates that the distribution is not yet fully deployed\.

   After your distribution is deployed, you can reference your content with the new CloudFront domain name\.

1. Record the value of **Domain Name** shown in the CloudFront console, for example, `dj4p1rv6mvubz.cloudfront.net`\. 

1. To verify that your CloudFront distribution is working, enter the domain name of the distribution in a web browser\.

   If your website is visible, the CloudFront distribution works\. If your website has a custom domain registered with Amazon Route 53, you will need the CloudFront domain name to update the record set in the next step\.

## Step 2: Update the record sets for your domain and subdomain<a name="update-record-sets"></a>

Now that you have successfully created a CloudFront distribution, update the alias record in Route 53 to point to the new CloudFront distribution\.

**To update the alias record to point to a CloudFront distribution**

1. Open the Route 53 console at [https://console\.aws\.amazon\.com/route53/](https://console.aws.amazon.com/route53/)\.

1. In the left navigation, choose **Hosted zones**\.

1. On the **Hosted Zones** page, choose the hosted zone that you created for your subdomain, for example, `www.example.com`\.

1. Under **Records**, select the *A* record that you created for your subdomain\. 

1. Under **Record details**, choose **Edit record**\.

1. Under **Route traffic to**, choose **Alias to CloudFront distribution**\.

1. Under **Choose distribution**, choose the CloudFront distribution\.

1. Choose **Save**\.

1. To redirect the *A* record for the root domain to the CloudFront distribution, repeat this procedure for the root domain, for example, `example.com`\.

   The update to the record sets takes effect within 2–48 hours\. 

1. To see whether the new *A* records have taken effect, in a web browser, enter your subdomain URL, for example, `http://www.example.com`\. 

   If the browser no longer redirects you to the root domain \(for example, `http://example.com`\), the new A records are in place\. When the new *A* record has taken effect, traffic routed by the new *A* record to the CloudFront distribution is not redirected to the root domain\. Any visitors who reference the site by using `http://example.com` or `http://www.example.com` are redirected to the nearest CloudFront edge location, where they benefit from faster download times\.
**Tip**  
Browsers can cache redirect settings\. If you think the new *A* record settings should have taken effect, but your browser still redirects `http://www.example.com` to `http://example.com`, try clearing your browser history and cache, closing and reopening your browser application, or using a different web browser\. 

## \(Optional\) Step 3: Check the log files<a name="check-log-files"></a>

The access logs tell you how many people are visiting the website\. They also contain valuable business data that you can analyze with other services, such as [Amazon EMR](https://docs.aws.amazon.com/emr/latest/DeveloperGuide/)\. 

CloudFront logs are stored in the bucket and folder that you choose when you create a CloudFront distribution and enable logging\. CloudFront writes logs to your log bucket within 24 hours from when the corresponding requests are made\.

**To see the log files for your website**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the name of the logging bucket for your website\.

1. Choose the CloudFront logs folder\.

1. Download the `.gzip` files written by CloudFront before opening them\.

   If you created your website only as a learning exercise, you can delete the resources that you allocated so that you no longer accrue charges\. To do so, see [Cleaning up your example resources](getting-started-cleanup.md)\. After you delete your AWS resources, your website is no longer available\.