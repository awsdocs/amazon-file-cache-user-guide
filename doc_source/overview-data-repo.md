# Overview of data repositories<a name="overview-data-repo"></a>

Amazon File Cache is well integrated with data repositories in Amazon S3 or on NFS \(Network File System\) file systems that support the NFSv3 protocol\. This integration means that you can seamlessly access the objects stored in your Amazon S3 buckets or NFS data repositories from applications that mount your cache\. You can also run your compute\-intensive workloads on Amazon EC2 instances in the AWS Cloud and export the results to your data repository after your workload is complete\.

**Note**  
You can link your cache to either S3 or NFS data repositories, but not to both types at the same time\. You cannot have a mix of linked S3 and NFS data repositories on a single cache\.

When you use Amazon File Cache with multiple storage repositories, you can ingest and process large volumes of file data in a high\-performance cache\.  At the same time, you can write results to your data repositories by using  HSM commands\. With these features, you can restart your workload at any time using the latest data stored in your data repository\.

By default, File Cache automatically loads data into the cache when it’s accessed for the first time \(lazy load\)\. You can optionally pre\-load data into the cache before starting your workload\.  For more information, see [Lazy load](mdll-lazy-load.md)\.

You can also export files and their associated metadata \(including POSIX metadata\) in your cache to your data repository using  HSM commands\.  When you use  HSM commands, file data and metadata that were created, or modified since the last such export are exported to the data repository\.  For more information, see [Exporting files using HSM commands](exporting-files-hsm.md)\.

**Important**  
If you have linked one or more caches to a data repository on Amazon S3, don't delete the Amazon S3 bucket until you have deleted all linked caches\.