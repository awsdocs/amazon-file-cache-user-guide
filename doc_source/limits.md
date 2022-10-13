# Quotas<a name="limits"></a>

Following, you can find out about quotas when working with Amazon File Cache\.

**Topics**
+ [Quotas that you can increase](#soft-limits)
+ [Resource quotas for each cache](#limits-file-cache-resources)
+ [Additional considerations](#limits-additional-considerations)

## Quotas that you can increase<a name="soft-limits"></a>

Following are the quotas for Amazon File Cache per AWS account, per AWS Region, which you can increase\.


****  

| Resource | Default | Description | 
| --- | --- | --- | 
|  Lustre Cache\_1 caches  |  100  |  The maximum number of Amazon File Cache caches with cache type `Lustre` and deployment type `Cache_1` that you can create in this account\.  | 
|  Lustre Cache\_1 storage capacity  |  100800  |  The maximum amount of storage capacity \(in GiB\) that you can configure in this account for all Amazon File Cache caches with cache type `Lustre` and deployment type `Cache_1`\.  | 

**To request a quota increase**

1. Open the [Service Quotas console](https://console.aws.amazon.com/servicequotas/home?region=us-east-1#!/dashboard)\.

1. In the navigation pane, choose **AWS services**\.

1. Choose **Amazon File Cache**\.

1. Choose a quota\.

1. Choose **Request quota increase**, and follow the directions to request a quota increase\.

1. To view the status of the quota request, choose **Quota request history** in the console navigation pane\.

For more information, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\.

## Resource quotas for each cache<a name="limits-file-cache-resources"></a>

Following are the limits on Amazon File Cache resources for each cache in an AWS Region\.


****  

| Resource | Limit per cache | 
| --- | --- | 
| Maximum number of tags | 50 | 
| Number of file updates from linked S3 bucket per caches | 10 million / month | 
| Minimum storage capacity | 1\.2 TiB | 
| Maximum throughput per unit of storage | 1000 MBps | 

## Additional considerations<a name="limits-additional-considerations"></a>

In addition, note the following:
+ You can use each AWS Key Management Service \(AWS KMS\) key on up to 125 Amazon File Cache caches\.
+ For a list of AWS Regions where you can create caches, see [Amazon File Cache availability](what-is.md#cache-availability)\.