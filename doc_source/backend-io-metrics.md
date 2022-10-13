# Back\-end I/O metrics<a name="backend-io-metrics"></a>

The following metrics report information on read and write operations between the cache and its linked data repository\. The metric takes one dimension \(`FileCacheId`\) and is published into the `AWS/FSx` namespace in CloudWatch\.


| Metric | Description | 
| --- | --- | 
| `RepositoryReadSuccess` | The rates of files being read into the cache from its linked data repository\. | 
| `RepositoryReadFailure` | The rate of files failing to be read into the cache from its linked data repositories\. If you observe a high rate of read failures, you can check if any data repository association \(DRA\) is unreachable or misconfigured and if the link between your VPC and an on\-premises data store is saturated\. If the link is not saturated, you can create a larger cache\.  | 
| `RepositoryWriteSuccess` | The rate of files being written from the cache to its linked data repositories\. | 
| `RepositoryMetadataReadSuccess` | Understand the rate at which the workload is triggering the loading of file and directory metadata into the cache\. | 
| `RepositoryMetadataReadFail` | The rate of file and directory metadata failing to be read into the cache from its linked data repositories\.\. If you have high metadata read failures, you check the connectivity to the linked data repository and shard the workload\.  | 
| `RepositoryMetadataReadNotFound` | The rate at which cache is looking up the data repository for paths that don't exist\. | 
| `RepositoryOperationsInProgress` | The number of read and write operations \(including metadata operations\) on data repositories that are in progress\. | 