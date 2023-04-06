# Viewing replication metrics by using the Amazon S3 console<a name="viewing-replication-metrics"></a>

There are three types of Amazon CloudWatch metrics for Amazon S3: storage metrics, request metrics, and replication metrics\. S3 Replication metrics are turned on automatically when you enable replication with S3 Replication Time Control \(S3 RTC\) by using the AWS Management Console or the Amazon S3 API\. You can also enable S3 Replication metrics independently of S3 RTC while creating or editing a rule\.

Replication metrics track the rule IDs of the replication configuration\. A replication rule ID can be specific to a prefix, a tag, or a combination of both\.

For more information about CloudWatch metrics for Amazon S3, see [Monitoring metrics with Amazon CloudWatch](cloudwatch-monitoring.md)\.

**Prerequisites**  
Enable a replication rule that has S3 Replication metrics\.

**To view replication metrics**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the left navigation pane, choose **Buckets**\. In the **Buckets** list, choose the name of the bucket that contains the objects that you want replication metrics for\.

1. Choose the **Metrics** tab\.

1. Under **Replication metrics**, choose **Replication rules**\.

1. Choose **Display charts**\.

   Amazon S3 displays **Replication latency \(in seconds\)**, **Bytes pending replication**, **Operations pending replication**, and **Operations failed replication** charts\.

You can then view the replication metrics **Replication latency \(in seconds\)**, **Operations pending replication**, **Bytes pending replication**, and **Operations failed replication** for the rules that you selected\. If you're using S3 Replication Time Control, Amazon CloudWatch begins reporting replication metrics 15 minutes after you enable S3 RTC on the respective replication rule\. You can view replication metrics on the Amazon S3 console or the CloudWatch console\. For more information, see [Replication metrics with S3 RTC](replication-time-control.md#using-replication-metrics)\.