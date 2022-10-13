# Amazon File Cache performance<a name="performance"></a>

Amazon File Cache is built on Lustre, the popular high\-performance file system, and provides scale\-out performance that increases linearly with a File Cache’s size\. Lustre file systems scale horizontally across multiple file servers and disks\. This scaling gives each client direct access to the data stored on each disk to remove many of the bottlenecks present in traditional file systems\. Amazon File Cache builds on Lustre's scalable architecture to support high levels of performance across large numbers of clients\.

**Topics**
+ [How Amazon File Cache works](#how-lustre-fs-work)
+ [Aggregate cache performance](#fsx-aggregate-perf)
+ [File storage layout](#storage-layout)
+ [Striping data in your cache](#striping-data)
+ [Monitoring performance and usage](#performance-monitoring)
+ [Performance tips](#performance-tips)

## How Amazon File Cache works<a name="how-lustre-fs-work"></a>

Each Amazon File Cache resource consists of the file servers that the clients communicate with, and a set of disks attached to each file server that stores your cache data\. Each file server employs a fast, in\-memory cache to enhance performance for the most frequently accessed data\.  When a client accesses data that's stored in the in\-memory  cache, the file server doesn't need to read it from disk, which reduces latency and increases the total amount of throughput you can drive\.

 When you read data that is stored on the file server's in\-memory  cache, your cache performance is determined by the network throughput\. When you write data to your cache, or when you read data that isn't stored on the in\-memory cache, your cache performance is determined by the lower of the network throughput and disk throughput\.

## Aggregate cache performance<a name="fsx-aggregate-perf"></a>

The throughput that an Amazon File Cache supports is proportional to its storage capacity\. Amazon File Cache scales to hundreds of GBps of throughput and millions of IOPS\. Amazon File Cache also supports concurrent access to the same file or directory from thousands of compute instances\. This access enables rapid data checkpointing from application memory to storage, which is a common technique in high performance computing \(HPC\)\.

The following table shows performance that the Amazon File Cache deployment type is designed for\.


**File Cache performance for SSD storage**  

| Deployment Type |  **Network throughput \(MB/s/TiB of storage provisioned\)** |  **Network IOPS \(IOPS/TiB of storage provisioned\)** |  **Cache storage \(GiB of RAM/TiB of storage provisioned\)** |  **Disk latencies per file operation \(milliseconds, P50\)** |  **Disk throughput \(MB/s/TiB of storage provisioned\)** | 
| --- |--- |--- |--- |--- |--- |
| CACHE\-1000 | 2600 | Tens of thousands baselineHundreds of thousands burst | 27\.3 |  Metadata: sub\-ms Data: sub\-ms  |  1000  | 
| --- |--- |--- |--- |--- |--- |

### Example: Aggregate baseline and burst throughput<a name="example-persistant-throughput"></a>

The following example illustrates how storage capacity and disk throughput impact cache performance\.

A cache with a storage capacity of 4\.8 TiB and 1000 MB/s per TiB of throughput per unit of storage provides an aggregate disk throughput of 4800 MB/s\.

Regardless of cache size, Amazon File Cache provides consistent, sub\-millisecond latencies for file operations\.

## File storage layout<a name="storage-layout"></a>

All file data in Lustre is stored on storage volumes called *object storage targets* \(OSTs\)\. All file metadata \(including file names, timestamps, permissions, and more\) is stored on storage volumes called *metadata targets* \(MDTs\)\. An Amazon File Cache is composed of multiple MDTs and OSTs\. Each OST is approximately 1\.2 TiB in size\. Amazon File Cache spreads your file data across the OSTs that make up your cache to balance storage capacity with throughput and IOPS load\.

To view the storage usage of the MDTs and OSTs that make up your cache, run the following command from a client that has the cache mounted\.

```
lfs df -h mount/path
```

The output of this command looks like the following\.

**Example**  

```
UUID                             bytes       Used   Available Use% Mounted on
mountname-MDT0000_UUID           68.7G       5.4M       68.7G   0% /fsx[MDT:0]
mountname-OST0000_UUID            1.1T       4.5M        1.1T   0% /fsx[OST:0]
mountname-OST0001_UUID            1.1T       4.5M        1.1T   0% /fsx[OST:1]

filesystem_summary:               2.2T       9.0M        2.2T   0% /fsx
```

## Striping data in your cache<a name="striping-data"></a>

You can optimize your cache's throughput performance with file striping\. Amazon File Cache automatically spreads out files across OSTs in order to ensure that data is served from all storage servers\. You can apply the same concept at the file level by configuring how files are striped across multiple OSTs\.

Striping means that files can be divided into multiple chunks that are then stored across different OSTs\. When a file is striped across multiple OSTs, read or write requests to the file are spread across those OSTs, increasing the aggregate throughput or IOPS your applications can drive through it\.

The default layout is a progressive file layout in which files under 1GiB in size are stored in one stripe, and larger files are assigned a stripe count of five\.

You can view the layout configuration of a file or directory using the `lfs getstripe` command\.

```
lfs getstripe path/to/filename
```

This command reports a file's stripe count, stripe size, and stripe offset\. The *stripe count * is how many OSTs the file is striped across\. The *stripe size *is how much continuous data is stored on an OST\. The *stripe offset *is the index of the first OST that the file is striped across\.

### Modifying your striping configuration<a name="striping-modify"></a>

A file's layout parameters are set when the file is first created\. Use the `lfs setstripe` command to create a new, empty file with a specified layout\.

```
lfs setstripe filename --stripe-count number_of_OSTs
```

The `lfs setstripe` command affects only the layout of a new file\. Use it to specify the layout of a file before you create it\. You can also define a layout for a directory\. Once set on a directory, that layout is applied to every new file added to that directory, but not to existing files\. Any new subdirectory you create also inherits the new layout, which is then applied to any new file or directory you create within that subdirectory\.

To modify the layout of an existing file, use the `lfs migrate` command\. This command copies the file as needed to distribute its content according to the layout you specify in the command\. For example, files that are appended to or are increased in size don't change the stripe count, so you have to migrate them to change the file layout\. Alternatively, you can create a new file using the `lfs setstripe` command to specify its layout, copy the original content to the new file, and then rename the new file to replace the original file\.

There may be cases where the default layout configuration is not optimal for your workload\. For example, a cache with tens of OSTs and a large number of multi\-gigabyte files may see higher performance by striping the files across more than the default stripe count value of five OSTs\. Creating large files with low stripe counts can cause I/O performance bottlenecks and can also cause OSTs to fill up\. In this case, you can create a directory with a larger stripe count for these files\.

Setting up a striped layout for large files \(especially files larger than a gigabyte in size\) is important for the following reasons:
+ Improves throughput by allowing multiple OSTs and their associated servers to contribute IOPS, network bandwidth, and CPU resources when reading and writing large files\.
+ Reduces the likelihood that a small subset of OSTs become hot spots that limit overall workload performance\.
+ Prevents a single large file from filling an OST, possibly causing disk full errors\.

There is no single optimal layout configuration for all use cases\. For detailed guidance on file layouts, see [Managing File Layout \(Striping\) and Free Space](https://doc.lustre.org/lustre_manual.xhtml#managingstripingfreespace) in the Lustre\.org documentation\. The following are general guidelines:
+ Striped layout matters most for large files, especially for use cases where files are routinely hundreds of megabytes or more in size\. For this reason, the default layout for a new cache assigns a striped count of five for files over 1GiB in size\.
+ Stripe count is the layout parameter that you should adjust for systems supporting large files\. The stripe count specifies the number of OST volumes that will hold chunks of a striped file\. For example, with a stripe count of 2 and a stripe size of 1MiB, Lustre writes alternate 1MiB chunks of a file to each of two OSTs\.
+ The effective stripe count is the lesser of the actual number of OST volumes and the stripe count value you specify\. You can use the special stripe count value of `-1` to indicate that stripes should be placed on all OST volumes\.
+ Setting a large stripe count for small files is sub\-optimal because for certain operations Lustre requires a network round trip to every OST in the layout, even if the file is too small to consume space on all the OST volumes\.
+ You can set up a progressive file layout \(PFL\) that allows the layout of a file to change with size\. A PFL configuration can simplify managing a cache that has a combination of large and small files without you having to explicitly set a configuration for each file\. For more information, see [Progressive file layouts](#striping-pfl)\.
+ Stripe size by default is 1MiB\. Setting a stripe offset may be useful in special circumstances, but in general it is best to leave it unspecified and use the default\.

### Progressive file layouts<a name="striping-pfl"></a>

You can specify a progressive file layout \(PFL\) configuration for a directory to specify different stripe configurations for small and large files before populating it\. For example, you can set a PFL on the top\-level directory before any data is written to a new cache\.

To specify a PFL configuration, use the `lfs setstripe` command with `-E` options to specify layout components for different sized files, such as the following command:

```
lfs setstripe -E 100M -c 1 -E 10G -c 8 -E -1 -c -1 /mountname/directory
```

This command sets three layout components:
+ The first component \(`-E 100M -c 1`\) indicates a stripe count value of 1 for files up to 100MiB in size\.
+ The second component \(`-E 10G -c 8`\) indicates a stripe count of 8 for files up to 10GiB in size\.
+ The third component \(`-E -1 -c -1`\) indicates that files larger than 10GiB will be striped across all OSTs\.

**Important**  
Appending data to a file created with a PFL layout will populate all of its layout components\. For example, with the 3\-component command shown above, if you create a 1MiB file and then add data to the end of it, the layout of the file will expand to have a stripe count of \-1, meaning all the OSTs in the system\. This does not mean data will be written to every OST, but an operation such as reading the file length will send a request in parallel to every OST, adding significant network load to the cache\.  
Therefore, be careful to limit the stripe count for any small or medium length file that can subsequently have data appended to it\. Because log files usually grow by having new records appended, Amazon File Cache assigns a default stripe count of 1 to any file created in append mode, regardless of the default stripe configuration specified by its parent directory\.

The default PFL configuration on caches is set with this command:

```
lfs setstripe -E 100M -c 1 -E 10G -c 8 -E 100G -c 16 -E -1 -c 32 /mountname
```

Customers with workloads that have highly concurrent access on medium and large files are likely to benefit from a layout with more stripes at smaller sizes and striping across all OSTs for the largest files, as shown in the three\-component example layout shown previously\.

## Monitoring performance and usage<a name="performance-monitoring"></a>

Every minute, Amazon File Cache emits usage metrics for each disk \(MDT and OST\) to Amazon CloudWatch\.

To view aggregate cache usage details, you can look at the Sum statistic of each metric\. For example, the Sum of the `DataReadBytes` statistic reports the total read throughput seen by all the OSTs in a cache\. Similarly, the Sum of the `FreeDataStorageCapacity` statistic reports the total available storage capacity for file data in the cache\.

For more information on monitoring your cache’s performance, see [Monitoring Amazon File Cache](monitoring_overview.md)\.

## Performance tips<a name="performance-tips"></a>

When using Amazon File Cache, keep the following performance tips in mind\. For service limits, see [Quotas](limits.md)\.
+ **Average I/O size** – Because Amazon File Cache is a network file system, each file operation goes through a round trip between the client and Amazon File Cache, incurring a small latency overhead\. Due to this per\-operation latency, overall throughput generally increases as the average I/O size increases, because the overhead is amortized over a larger amount of data\.
+ **Request model** – By enabling asynchronous writes to your cache, pending write operations are buffered on the Amazon EC2 instance before they are written to Amazon File Cache asynchronously\. Asynchronous writes typically have lower latencies\. When performing asynchronous writes, the kernel uses additional memory for caching\. A cache that has enabled synchronous writes issues synchronous requests to Amazon File Cache\. Every operation goes through a round trip between the client and Amazon File Cache\.
**Note**  
Your chosen request model has tradeoffs in consistency \(if you're using multiple Amazon EC2 instances\) and speed\.
+ **Amazon EC2 instances** – Applications that perform a large number of read and write operations likely need more memory or computing capacity than applications that don't\. When launching your Amazon EC2 instances for your compute\-intensive workload, choose instance types that have the amount of these resources that your application needs\. The performance characteristics of Amazon File Cache don't depend on the use of Amazon EBS–optimized instances\.
+ **Recommended tuning for large client instance types**

  1. To tune large client instances for optimal performance: 

     1. For client instance types with memory of more than 64 GiB, we recommend applying the following tuning:

        ```
        lctl set_param ldlm.namespaces.*.lru_max_age=600000
        ```

     1. For client instance types with more than 64 CPU cores, we recommend applying the following tuning:

        ```
        echo "options ptlrpc ptlrpcd_per_cpt_max=32" >> /etc/modprobe.d/modprobe.conf
        echo "options ksocklnd credits=2560" >> /etc/modprobe.d/modprobe.conf
                    
        # reload all kernel modules to apply the above two settings
        sudo reboot
        ```

  1. After the client is mounted, the following tuning needs to be applied:

     ```
     sudo lctl set_param osc.*OST*.max_rpcs_in_flight=32
     sudo lctl set_param mdc.*.max_rpcs_in_flight=64
     sudo lctl set_param mdc.*.max_mod_rpcs_in_flight=50
     ```

  Note that `lctl set_param` is known to not persist over reboot\. Since these parameters cannot be set permanently from the client side, it is recommended to implement a boot cron job to set the configuration with the recommended tunings\.
+ **Workload balance across OSTs** – In some cases, your workload isn’t driving the aggregate throughput that your cache can provide \(1000 MB/s per TiB of storage\)\. If so, you can use CloudWatch metrics to troubleshoot if performance is affected by an imbalance in your workload’s I/O patterns\. To identify if this is the cause, look at the Maximum `DataReadBytes` and `DataWriteBytes` CloudWatch metrics for Amazon File Cache\.

  In some cases, this statistic shows a load at or above 1200 MBps of throughput \(the throughput capacity of a single 1\.2\-TiB Amazon File Cache disk\)\. In such cases, your workload is not evenly spread out across your disks\. If this is the case, you can use the `lfs setstripe` command to modify the striping of files your workload is most frequently accessing\. For optimal performance, stripe files with high throughput requirements across all the OSTs comprising your cache\.