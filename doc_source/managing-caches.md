# Managing caches<a name="managing-caches"></a>

A *cache* is the primary Amazon File Cache resource\. You can create, list, update, and delete an Amazon File Cache using the AWS Management Console, AWS CLI, and API\.

## Creating caches<a name="creating-file-systems"></a>

You can create an Amazon File Cache using the AWS Management Console, AWS CLI, or the AWS API\.

**Note**  
You can link your cache to data repositories only when you create the cache\. You can link up to 8 data repositories, which must all be of the same repository type \(all S3 or all NFS\)\.

### To create a cache \(console\)<a name="create-file-system-console"></a>
+ For information about how to create a cache from the console, see [Step 1: Create your cache](getting-started-step1.md)\.

### To create a cache \(CLI\)<a name="create-file-system-cli"></a>
+ To create an Amazon File Cache, use the [create\-file\-cache](https://docs.aws.amazon.com/cli/latest/reference/fsx/create-file-cache.html) CLI command \(or the equivalent [CreateFileCache](https://docs.aws.amazon.com/fsx/latest/APIReference/API_CreateFileCache.html) API operation\)\. The following example creates an Amazon File Cache\. Use the `--data-repository-associations` option to specify links to up go 8 data repositories\. This example creates links to 3 Amazon S3 buckets\.

  ```
  fsx create-file-cache \
  --storage-capacity 1200 \
  --subnet-ids subnet-0156f2909ff21e78b \
  --security-group-ids sg-0123456789abcdef3 \
  --file-cache-type LUSTRE \
  --file-cache-type-version 2.12 \
  --lustre-configuration "DeploymentType=CACHE_1,PerUnitStorageThroughput=1000,MetadataConfiguration={StorageCapacity=2400}" \
  --data-repository-associations FileCachePath=/ns2/,DataRepositoryPath=s3://s3cache2/fsx/ \
    FileCachePath=/ns3/,DataRepositoryPath=s3://s3cache3/fsx/ \
    FileCachePath=/ns4/,DataRepositoryPath=s3://s3cache4/fsx/
  ```

After successfully creating the cache, Amazon File Cache returns the cache description in JSON format, as shown in the following example\.

```
{
    "FileCache": {
        "OwnerId": "1234567890120",
        "CreationTime": 1661841150.136,
        "FileCacheId": "fc-0123456789abcdef0",
        "FileCacheType": "LUSTRE",
        "FileCacheTypeVersion": "2.12",
        "Lifecycle": "CREATING",
        "StorageCapacity": 1200,
        "VpcId": "vpc-0222d95ad3328a351",
        "SubnetIds": [
            "sg-0123456789abcdef3"
        ],
        "DNSName": "fc-0123456789abcdef0.fsx.us-east-1.amazonaws.com",
        "KmsKeyId": "arn:aws:kms:us-east-1:123456789012:key/71726ts-r4sd-98gs-9ist-35w7tbs",
        "ResourceARN": "arn:aws:fsx:us-east-1:123456789012:file-cache/fc-0123456789abcdef0",
        "Tags": [],
        "CopyTagsToDataRepositoryAssociations": false,
        "LustreConfiguration": {
            "PerUnitStorageThroughput": 1000,
            "DeploymentType": "CACHE_1",
            "MountName": "fmvszbmv",
            "WeeklyMaintenanceStartTime": "5:06:00",
            "MetadataConfiguration": {
                "StorageCapacity": 2400
            },
            "LogConfiguration": {
                "Level": "WARN_ERROR",
                "Destination": "arn:aws:logs:us-east-1:123456789012:log-group:/aws/fsx/filecache/lustre:log-stream:datarepo_fc-0123456789abcdef0"
            }
        },
        "DataRepositoryAssociationIds": [
            "dra-04d15aa012bdcf0f8",
            "dra-036182e206928928c",
            "dra-07a25a1250582261e"
        ]
    }
}
```

## Updating caches<a name="updating-file-system"></a>

You can update the **Weekly maintenance window** configuration that sets the day of the week and time that Amazon File Cache performs cache maintenance and updates\. You can update the configuration of an Amazon File Cache resource using the AWS Management Console, the AWS CLI, and the AWS API\.



### To update a cache \(console\)<a name="update-file-system-console"></a>
+ For information about how to change the configuration of the weekly maintenance window, see [Amazon File Cache maintenance windows](maintenance-windows.md)\.

### To update a cache \(CLI\)<a name="update-file-system-cli"></a>
+ To update the configuration of an Amazon File Cache, use the [update\-file\-cache](https://docs.aws.amazon.com/cli/latest/reference/fsx/update-file-cache.html) CLI command \(or the equivalent [UpdateFileCache](https://docs.aws.amazon.com/fsx/latest/APIReference/API_UpdateFileCache.html) API operation\), as shown in the following example\.

  ```
  aws fsx update-file-cache \
      --file-cache-id fc-0123456789abcdef0 \
      --lustre-configuration WeeklyMaintenanceStartTime=1:01:30
  ```

## Deleting caches<a name="delete-file-system"></a>

You can delete an Amazon File Cache using the AWS Management Console, the AWS CLI, and the AWS API and SDKs\.

**To delete a cache:**
+ **Using the console** – Follow the procedure described in [Step 4: Clean up resources](getting-started-step4.md)\.
+ **Using the CLI or API** – Use the [delete\-file\-cache](https://docs.aws.amazon.com/cli/latest/reference/fsx/delete-file-cache.html) CLI command or the [DeleteFileCache](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DeleteFileCache.html) API operation\. The following example shows how to use the CLI to delete an Amazon File Cache resource\.

  ```
  aws fsx delete-file-cache --file-cache-id fc-0123456789abcdef0
  ```

## Viewing caches<a name="viewing-file-system"></a>

You can view the details of your cache using the AWS Management Console, the AWS CLI, and the AWS API and SDKs\.

**To view a cache:**
+ **Using the console** – Choose a cache to view the cache detail page\. The **Summary** panel shows the cache ID, lifecycle state, cache type, deployment type, storage type, storage capacity, metadata storage capacity, throughput per unit of storage, total throughput, Lustre version, availability zones, creation time, and mount name\.

  The tabs provide detailed information for the cache's features, such as your linked data repositories\.
+ **Using the CLI or API **– Use the [describe\-file\-caches](https://docs.aws.amazon.com/cli/latest/reference/fsx/describe-file-caches.html) CLI command or the [DescribeFileCaches](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DescribeFileCaches.html) API operation\. The following example shows how to use the CLI to return the description of a specific Amazon File Cache\.

  ```
  aws fsx describe-file-caches \
      --file-cache-ids fc-0123456789abcdef0
  ```

  To return descriptions of all existing caches, omit the `--file-cache-ids` option\.

## Amazon File Cache status<a name="file-system-lifecycle-states"></a>

You can view the status of a cache by using the AWS Management Console, the AWS CLI command [describe\-caches](https://docs.aws.amazon.com/cli/latest/reference/fsx/describe-file-caches.html), or the API operation [DescribeFileCaches](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DescribeFileCaches.html)\.


| Cache status  | Description | 
| --- | --- | 
| AVAILABLE | The cache has been successfully created and is available for use\. | 
| CREATING | A new cache is being created\. | 
| DELETING | An existing cache is being deleted\. | 
| UPDATING | The cache is undergoing a customer\-initiated update\. | 
| FAILED |  This status can mean either of the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/fsx/latest/FileCacheGuide/managing-caches.html)  | 