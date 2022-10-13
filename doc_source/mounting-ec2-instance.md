# Mounting from an Amazon Elastic Compute Cloud instance<a name="mounting-ec2-instance"></a>

You can mount your cache from an Amazon EC2 instance\.

**To mount your cache from Amazon EC2**

1. Connect to your Amazon EC2 instance\.

1. Make a directory on your cache for the mount point with the following command\.

   ```
   $ sudo mkdir -p /mnt
   ```

1. Mount the cache to the directory that you created\. Use the following command and replace the following items:
   + Replace `cache_dns_name` with the actual file cache's DNS name\.
   + Replace `mountname` with the cache's mount name\. This mount name is returned in the `CreateFileCache` API operation response\. It's also returned in the response of the describe\-file\-caches AWS CLI command, and the [DescribeFileCaches](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DescribeFileCaches.html) API operation\.

   ```
   sudo mount -t lustre -o relatime,flock cache_dns_name@tcp:/mountname /mnt
   ```

    This command mounts your cache with these options:
   +  `relatime` – Maintains `atime` \(inode access times\) data, but not for each time that a file is accessed\. With this option enabled, `atime` data is written to disk only if the file has been modified since the `atime` data was last updated \(mtime\), or if the file was last accessed more than a certain amount of time ago \(one day by default\)\. `relatime` is required for [automatic cache eviction](cache-eviction.md#auto-cache-eviction) to work properly\.
   +  `flock` – Enables file locking for your cache\. If you don't want file locking enabled, use the `mount` command without `flock`\. 

1. Verify that the mount command was successful by listing the contents of the directory to which you mounted the cache, `/mnt` by using the following command\.

   ```
   $ ls /mnt
   import-path  lustre
   $
   ```

   You can also use the `df` command, following\.

   ```
   $ df
   Filesystem                    1K-blocks    Used  Available Use% Mounted on
   devtmpfs                        1001808       0    1001808   0% /dev
   tmpfs                           1019760       0    1019760   0% /dev/shm
   tmpfs                           1019760     392    1019368   1% /run
   tmpfs                           1019760       0    1019760   0% /sys/fs/cgroup
   /dev/xvda1                      8376300 1263180    7113120  16% /
   123.456.789.0@tcp:/mountname 3547698816   13824 3547678848   1% /mnt
   tmpfs                            203956       0     203956   0% /run/user/1000
   ```

   The results show Amazon File Cache mounted on `/mnt`\.