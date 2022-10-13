# Mounting specific filesets<a name="mounting-from-fileset"></a>

By using the Lustre fileset feature, you can mount only a subset of the cache namespace, which is called a *fileset*\. To mount a fileset of the cache, on the client you specify the subdirectory path after the cache name\. A fileset mount \(also called a subdirectory mount\) limits the cache namespace visibility on a specific client\.

**Example – Mount a Lustre fileset**

1. Assume you have a cache with the following directories:

   ```
   team1/dataset1/
   team2/dataset2/
   ```

1. You mount only the `team1/dataset1` fileset, making only this part of the cache visible locally on the client\. Use the following command and replace the following items:
   + Replace `cache_dns_name` with the actual cache's DNS name\.
   + Replace `mountname` with the cache's mount name\. This mount name is returned in the `CreateFileCache` API operation response\. It's also returned in the response of the describe\-file\-caches AWS CLI command, and the [DescribeFileCaches](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DescribeFileCaches.html) API operation\.

   ```
   mount -t lustre -o relatime,flock cache_dns_name@tcp:/mountname/team1/dataset1 /mnt
   ```

When using the Lustre fileset feature, keep the following in mind:
+ Before a cache directory can be mounted on a client, you must list the directory by running the `ls` command on a parent directory in the cache\.
+ There are no constraints preventing a client from remounting the cache using a different fileset, or no fileset at all\.
+ When using a fileset, some Lustre administrative commands requiring access to the `.lustre/` directory may not work, such as the `lfs fid2path` command\.
+ If you plan to mount several subdirectories from the same cache on the same host, be aware that this consumes more resources than a single mount point, and it could be more efficient to mount the cache root directory only once instead\.

For more information on the Lustre fileset feature, see the *Lustre Operations Manual* on the [Lustre documentation website](https://doc.lustre.org/lustre_manual.xhtml#SystemConfigurationUtilities.fileset)\.