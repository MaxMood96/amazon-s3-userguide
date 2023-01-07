# Amazon S3 server access log format<a name="LogFormat"></a>

Server access logging provides detailed records for the requests that are made to an Amazon S3 bucket\. You can use server access logs for the following purposes: 
+ Performing security and access audits
+ Learning about your customer base
+ Understanding your Amazon S3 bill

This section describes the format and other details about Amazon S3 server access log files\.

Server access log files consist of a sequence of newline\-delimited log records\. Each log record represents one request and consists of space\-delimited fields\. 

The following is an example log consisting of five log records\. 

```
79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be DOC-EXAMPLE-BUCKET1 [06/Feb/2019:00:00:38 +0000] 192.0.2.3 79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be 3E57427F3EXAMPLE REST.GET.VERSIONING - "GET /DOC-EXAMPLE-BUCKET1?versioning HTTP/1.1" 200 - 113 - 7 - "-" "S3Console/0.4" - s9lzHYrFp76ZVxRcpX9+5cjAnEH2ROuNkd2BHfIa6UkFVdtjf5mKR3/eTPFvsiP/XV/VLi31234= SigV4 ECDHE-RSA-AES128-GCM-SHA256 AuthHeader DOC-EXAMPLE-BUCKET1.s3.us-west-1.amazonaws.com TLSV1.2 arn:aws:s3:us-west-1:123456789012:accesspoint/example-AP Yes
79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be DOC-EXAMPLE-BUCKET1 [06/Feb/2019:00:00:38 +0000] 192.0.2.3 79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be 891CE47D2EXAMPLE REST.GET.LOGGING_STATUS - "GET /DOC-EXAMPLE-BUCKET1?logging HTTP/1.1" 200 - 242 - 11 - "-" "S3Console/0.4" - 9vKBE6vMhrNiWHZmb2L0mXOcqPGzQOI5XLnCtZNPxev+Hf+7tpT6sxDwDty4LHBUOZJG96N1234= SigV4 ECDHE-RSA-AES128-GCM-SHA256 AuthHeader DOC-EXAMPLE-BUCKET1.s3.us-west-1.amazonaws.com TLSV1.2 - -
79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be DOC-EXAMPLE-BUCKET1 [06/Feb/2019:00:00:38 +0000] 192.0.2.3 79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be A1206F460EXAMPLE REST.GET.BUCKETPOLICY - "GET /DOC-EXAMPLE-BUCKET1?policy HTTP/1.1" 404 NoSuchBucketPolicy 297 - 38 - "-" "S3Console/0.4" - BNaBsXZQQDbssi6xMBdBU2sLt+Yf5kZDmeBUP35sFoKa3sLLeMC78iwEIWxs99CRUrbS4n11234= SigV4 ECDHE-RSA-AES128-GCM-SHA256 AuthHeader DOC-EXAMPLE-BUCKET1.s3.us-west-1.amazonaws.com TLSV1.2 - Yes 
79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be DOC-EXAMPLE-BUCKET1 [06/Feb/2019:00:01:00 +0000] 192.0.2.3 79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be 7B4A0FABBEXAMPLE REST.GET.VERSIONING - "GET /DOC-EXAMPLE-BUCKET1?versioning HTTP/1.1" 200 - 113 - 33 - "-" "S3Console/0.4" - Ke1bUcazaN1jWuUlPJaxF64cQVpUEhoZKEG/hmy/gijN/I1DeWqDfFvnpybfEseEME/u7ME1234= SigV4 ECDHE-RSA-AES128-GCM-SHA256 AuthHeader DOC-EXAMPLE-BUCKET1.s3.us-west-1.amazonaws.com TLSV1.2 - -
79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be DOC-EXAMPLE-BUCKET1 [06/Feb/2019:00:01:57 +0000] 192.0.2.3 79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be DD6CC733AEXAMPLE REST.PUT.OBJECT s3-dg.pdf "PUT /DOC-EXAMPLE-BUCKET1/s3-dg.pdf HTTP/1.1" 200 - - 4406583 41754 28 "-" "S3Console/0.4" - 10S62Zv81kBW7BB6SX4XJ48o6kpcl6LPwEoizZQQxJd5qDSCTLX0TgS37kYUBKQW3+bPdrg1234= SigV4 ECDHE-RSA-AES128-SHA AuthHeader DOC-EXAMPLE-BUCKET1.s3.us-west-1.amazonaws.com TLSV1.2 - Yes
```

**Note**  
Any field can be set to `-` to indicate that the data was unknown or unavailable, or that the field was not applicable to this request\. 

**Topics**
+ [Log record fields](#log-record-fields)
+ [Additional logging for copy operations](#AdditionalLoggingforCopyOperations)
+ [Custom access log information](#LogFormatCustom)
+ [Programming considerations for extensible server access log format](#LogFormatExtensible)

## Log record fields<a name="log-record-fields"></a>

The following list describes the log record fields\.

**Bucket Owner**  
The canonical user ID of the owner of the source bucket\. The canonical user ID is another form of the AWS account ID\. For more information about the canonical user ID, see [AWS account identifiers](https://docs.aws.amazon.com/general/latest/gr/acct-identifiers.html) in the *AWS General Reference*\. For information about how to find the canonical user ID for your account, see [Finding the canonical user ID for your AWS account](https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-identifiers.html#FindCanonicalId)\.  
**Example entry**  

```
79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be
```

**Bucket**  
The name of the bucket that the request was processed against\. If the system receives a malformed request and cannot determine the bucket, the request will not appear in any server access log\.  
**Example entry**  

```
DOC-EXAMPLE-BUCKET1
```

**Time**  
The time at which the request was received; these dates and times are in Coordinated Universal Time \(UTC\)\. The format, using `strftime()` terminology, is as follows: `[%d/%b/%Y:%H:%M:%S %z]`  
**Example entry**  

```
[06/Feb/2019:00:00:38 +0000]
```

**Remote IP**  
The apparent IP address of the requester\. Intermediate proxies and firewalls might obscure the actual IP address of the machine that's making the request\.  
**Example entry**  

```
192.0.2.3
```

**Requester**  
The canonical user ID of the requester, or a `-` for unauthenticated requests\. If the requester was an AWS Identity and Access Management \(IAM\) user, this field returns the requester's IAM user name along with the AWS root account that the IAM user belongs to\. This identifier is the same one used for access control purposes\.  
**Example entry**  

```
79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be
```

**Request ID**  
A string generated by Amazon S3 to uniquely identify each request\.  
**Example entry**  

```
3E57427F33A59F07
```

**Operation**  
The operation listed here is declared as `SOAP.operation`, `REST.HTTP_method.resource_type`, `WEBSITE.HTTP_method.resource_type`, or `BATCH.DELETE.OBJECT`, or `S3.action.resource_type` for [Lifecycle and logging](lifecycle-and-other-bucket-config.md#lifecycle-general-considerations-logging)\.  
**Example entry**  

```
REST.PUT.OBJECT
```

**Key**  
The key \(object name\) part of the request, URL encoded, or `-` if the operation does not take a key parameter\.  
**Example entry**  

```
/photos/2019/08/puppy.jpg
```

**Request\-URI**  
The `Request-URI` part of the HTTP request message\.  
**Example Entry**  

```
"GET /DOC-EXAMPLE-BUCKET1/photos/2019/08/puppy.jpg?x-foo=bar HTTP/1.1"
```

**HTTP status**  
The numeric HTTP status code of the response\.  
**Example entry**  

```
200
```

**Error Code**  
The Amazon S3 [Error code](UsingRESTError.md#ErrorCode), or `-` if no error occurred\.  
**Example entry**  

```
NoSuchBucket
```

**Bytes Sent**  
The number of response bytes sent, excluding HTTP protocol overhead, or `-` if zero\.  
**Example entry**  

```
2662992
```

**Object Size**  
The total size of the object in question\.  
**Example entry**  

```
3462992
```

**Total Time**  
The number of milliseconds that the request was in flight from the server's perspective\. This value is measured from the time that your request is received to the time that the last byte of the response is sent\. Measurements made from the client's perspective might be longer because of network latency\.  
**Example entry**  

```
70
```

**Turn\-Around Time**  
The number of milliseconds that Amazon S3 spent processing your request\. This value is measured from the time that the last byte of your request was received until the time that the first byte of the response was sent\.  
**Example entry**  

```
10
```

**Referer**  
The value of the HTTP `Referer` header, if present\. HTTP user\-agents \(for example, browsers\) typically set this header to the URL of the linking or embedding page when making a request\.  
**Example entry**  

```
"http://www.example.com/webservices"
```

**User\-Agent**  
The value of the HTTP `User-Agent` header\.  
**Example entry**  

```
"curl/7.15.1"
```

**Version Id**  
The version ID in the request, or `-` if the operation doesn't take a `versionId` parameter\.  
**Example entry**  

```
3HL4kqtJvjVBH40Nrjfkd
```

**Host Id**  
The `x-amz-id-2` or Amazon S3 extended request ID\.   
**Example entry**  

```
s9lzHYrFp76ZVxRcpX9+5cjAnEH2ROuNkd2BHfIa6UkFVdtjf5mKR3/eTPFvsiP/XV/VLi31234=
```

**Signature Version**  
The signature version, `SigV2` or `SigV4`, that was used to authenticate the request or a `-` for unauthenticated requests\.  
**Example entry**  

```
SigV2
```

**Cipher Suite**  
The Secure Sockets Layer \(SSL\) cipher that was negotiated for an HTTPS request or a `-` for HTTP\.  
**Example entry**  

```
ECDHE-RSA-AES128-GCM-SHA256
```

**Authentication Type**  
The type of request authentication used: `AuthHeader` for authentication headers, `QueryString` for query string \(presigned URL\), or a `-` for unauthenticated requests\.  
**Example entry**  

```
AuthHeader
```

**Host Header**  
The endpoint used to connect to Amazon S3\.  
**Example entry**  

```
s3.us-west-2.amazonaws.com
```
Some earlier Regions support legacy endpoints\. You might see these endpoints in your server access logs or AWS CloudTrail logs\. For more information, see [Legacy endpoints](VirtualHosting.md#s3-legacy-endpoints)\. For a complete list of Amazon S3 Regions and endpoints, see [Amazon S3 endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/s3.html) in the *Amazon Web Services General Reference*\.

**TLS version**  
The Transport Layer Security \(TLS\) version negotiated by the client\. The value is one of following: `TLSv1`, `TLSv1.1`, `TLSv1.2`, or `-` if TLS wasn't used\.  
**Example entry**  

```
TLSv1.2
```

**Access Point ARN**  
The Amazon Resource Name \(ARN\) of the access point of the request\. If the access point ARN is malformed or not used, the field will contain a `-`\. For more information about access points, see [Using access points](using-access-points.md)\. For more information about ARNs, see [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *AWS Reference Guide*\.  
**Example entry**  

```
arn:aws:s3:us-east-1:123456789012:accesspoint/example-AP
```

**aclRequired**  
A string that indicates whether the request required an access control list \(ACL\) for authorization\. If the request required an ACL for authorization, the string is `Yes`\. If no ACLs were required, the string is `-`\. For more information about ACLs, see [Access control list \(ACL\) overview](acl-overview.md)\. For more information about using the `aclRequired` field to disable ACLs, see [Controlling ownership of objects and disabling ACLs for your bucket](about-object-ownership.md)\.   
**Example entry**  

```
Yes
```

## Additional logging for copy operations<a name="AdditionalLoggingforCopyOperations"></a>

A copy operation involves a `GET` and a `PUT`\. For that reason, we log two records when performing a copy operation\. The previous section describes the fields related to the `PUT` part of the operation\. The following list describes the fields in the record that relate to the `GET` part of the copy operation\.

**Bucket Owner**  
The canonical user ID of the bucket that stores the object being copied\. The canonical user ID is another form of the AWS account ID\. For more information about the canonical user ID, see [AWS account identifiers](https://docs.aws.amazon.com/general/latest/gr/acct-identifiers.html) in the *AWS General Reference*\. For information about how to find the canonical user ID for your account, see [Finding the canonical user ID for your AWS account](https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-identifiers.html#FindCanonicalId)\.  
**Example entry**  

```
79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be
```

**Bucket**  
The name of the bucket that stores the object that's being copied\.  
**Example entry**  

```
DOC-EXAMPLE-BUCKET1
```

**Time**  
The time at which the request was received; these dates and times are in Coordinated Universal Time \(UTC\)\. The format, using `strftime()` terminology, is as follows: `[%d/%B/%Y:%H:%M:%S %z]`  
**Example entry**  

```
[06/Feb/2019:00:00:38 +0000]
```

**Remote IP**  
The apparent IP address of the requester\. Intermediate proxies and firewalls might obscure the actual IP address of the machine that's making the request\.  
**Example entry**  

```
192.0.2.3
```

**Requester**  
The canonical user ID of the requester, or a `-` for unauthenticated requests\. If the requester was an IAM user, this field will return the requester's IAM user name along with the AWS root account that the IAM user belongs to\. This identifier is the same one used for access control purposes\.  
**Example entry**  

```
79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be
```

**Request ID**  
A string generated by Amazon S3 to uniquely identify each request\.  
**Example entry**  

```
3E57427F33A59F07
```

**Operation**  
The operation listed here is declared as `SOAP.operation`, `REST.HTTP_method.resource_type`, `WEBSITE.HTTP_method.resource_type`, or `BATCH.DELETE.OBJECT`\.  
**Example entry**  

```
REST.COPY.OBJECT_GET
```

**Key**  
The key \(object name\) of the object being copied, or `-` if the operation doesn't take a key parameter\.   
**Example entry**  

```
/photos/2019/08/puppy.jpg
```

**Request\-URI**  
The `Request-URI` part of the HTTP request message\.  
**Example entry**  

```
"GET /DOC-EXAMPLE-BUCKET1/photos/2019/08/puppy.jpg?x-foo=bar"
```

**HTTP status**  
The numeric HTTP status code of the `GET` portion of the copy operation\.  
**Example entry**  

```
200
```

**Error Code**  
The Amazon S3 [Error code](UsingRESTError.md#ErrorCode) of the `GET` portion of the copy operation, or `-` if no error occurred\.  
**Example entry**  

```
NoSuchBucket
```

**Bytes Sent**  
The number of response bytes sent, excluding the HTTP protocol overhead, or `-` if zero\.  
**Example entry**  

```
2662992
```

**Object Size**  
The total size of the object in question\.  
**Example entry**  

```
3462992
```

**Total Time**  
The number of milliseconds that the request was in flight from the server's perspective\. This value is measured from the time that your request is received to the time that the last byte of the response is sent\. Measurements made from the client's perspective might be longer because of network latency\.  
**Example entry**  

```
70
```

**Turn\-Around Time**  
The number of milliseconds that Amazon S3 spent processing your request\. This value is measured from the time that the last byte of your request was received until the time that the first byte of the response was sent\.  
**Example entry**  

```
10
```

**Referer**  
The value of the HTTP `Referer` header, if present\. HTTP user\-agents \(for example, browsers\) typically set this header to the URL of the linking or embedding page when making a request\.  
**Example entry**  

```
"http://www.example.com/webservices"
```

**User\-Agent**  
The value of the HTTP `User-Agent` header\.  
**Example entry**  

```
"curl/7.15.1"
```

**Version Id**  
The version ID of the object being copied, or `-` if the `x-amz-copy-source` header didn't specify a `versionId` parameter as part of the copy source\.  
**Example Entry**  

```
3HL4kqtJvjVBH40Nrjfkd
```

**Host Id**  
The `x-amz-id-2` or Amazon S3 extended request ID\.  
**Example entry**  

```
s9lzHYrFp76ZVxRcpX9+5cjAnEH2ROuNkd2BHfIa6UkFVdtjf5mKR3/eTPFvsiP/XV/VLi31234=
```

**Signature Version**  
The signature version, `SigV2` or `SigV4`, that was used to authenticate the request, or a `-` for unauthenticated requests\.  
**Example entry**  

```
SigV4
```

**Cipher Suite**  
The Secure Sockets Layer \(SSL\) cipher that was negotiated for an HTTPS request, or a `-` for HTTP\.  
**Example entry**  

```
ECDHE-RSA-AES128-GCM-SHA256
```

**Authentication Type**  
The type of request authentication used: `AuthHeader` for authentication headers, `QueryString` for query strings \(presigned URLs\), or a `-` for unauthenticated requests\.  
**Example entry**  

```
AuthHeader
```

**Host Header**  
The endpoint that was used to connect to Amazon S3\.  
**Example entry**  

```
s3.us-west-2.amazonaws.com
```
Some earlier Regions support legacy endpoints\. You might see these endpoints in your server access logs or AWS CloudTrail logs\. For more information, see [Legacy endpoints](VirtualHosting.md#s3-legacy-endpoints)\. For a complete list of Amazon S3 Regions and endpoints, see [Amazon S3 endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/s3.html) in the *Amazon Web Services General Reference*\.

**TLS version**  
The Transport Layer Security \(TLS\) version negotiated by the client\. The value is one of following: `TLSv1`, `TLSv1.1`, `TLSv1.2`, or `-` if TLS wasn't used\.  
**Example entry**  

```
TLSv1.2
```

**Access Point ARN**  
The Amazon Resource Name \(ARN\) of the access point of the request\. If the access point ARN is malformed or not used, the field will contain a `-`\. For more information about access points, see [Using access points](using-access-points.md)\. For more information about ARNs, see [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *AWS Reference Guide*\.  
**Example entry**  

```
arn:aws:s3:us-east-1:123456789012:accesspoint/example-AP
```

**aclRequired**  
A string that indicates whether the request required an access control list \(ACL\) for authorization\. If the request required an ACL for authorization, the string is `Yes`\. If no ACLs were required, the string is `-`\. For more information about ACLs, see [Access control list \(ACL\) overview](acl-overview.md)\. For more information about using the `aclRequired` field to disable ACLs, see [Controlling ownership of objects and disabling ACLs for your bucket](about-object-ownership.md)\.   
**Example entry**  

```
Yes
```

## Custom access log information<a name="LogFormatCustom"></a>

You can include custom information to be stored in the access log record for a request\. To do this, add a custom query\-string parameter to the URL for the request\. Amazon S3 ignores query\-string parameters that begin with `x-`, but includes those parameters in the access log record for the request, as part of the `Request-URI` field of the log record\. 

For example, a `GET` request for `"s3.amazonaws.com/DOC-EXAMPLE-BUCKET1/photos/2019/08/puppy.jpg?x-user=johndoe"` works the same as the request for `"s3.amazonaws.com/DOC-EXAMPLE-BUCKET1/photos/2019/08/puppy.jpg"`, except that the `"x-user=johndoe"` string is included in the `Request-URI` field for the associated log record\. This functionality is available in the REST interface only\.

## Programming considerations for extensible server access log format<a name="LogFormatExtensible"></a>

Occasionally, we might extend the access log record format by adding new fields to the end of each line\. Therefore, make sure that any of your code that parses server access logs can handle trailing fields that it might not understand\. 