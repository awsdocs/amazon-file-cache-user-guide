# Mounting your cache automatically<a name="mount-fs-auto-mount-onreboot"></a>

 You can update the `/etc/fstab` file in your Amazon EC2 instance after you connect to the instance for the first time so that it mounts your cache each time it reboots\.

## Using /etc/fstab to mount Amazon File Cache automatically<a name="lustre-mount-fs-auto-mount-update-fstab"></a>

To automatically mount your cache directory when the Amazon EC2 instance reboots, you can use the `fstab` file\. The `fstab` file contains information about the cache\. The command `mount -a`, which runs during instance startup, mounts the caches listed in the `fstab` file\.

**Note**  
Before you can update the `/etc/fstab` file of your EC2 instance, make sure that you've already created your cache\. For more information, see [Step 1: Create your cache](getting-started-step1.md) in the Getting Started exercise\.

**To update the /etc/fstab file in your EC2 instance**

1. Connect to your EC2 instance, and open the `/etc/fstab` file in an editor\.

1. Add the following line to the `/etc/fstab` file\.

   Mount Amazon File Cache to the directory that you created\. Use the following command and replace the following:
   + Replace *`/mnt`* with the directory that you want to mount your cache to\.
   + Replace `cache_dns_name` with the actual cache's DNS name\.
   + Replace `mountname` with the cache's mount name\. This mount name is returned in the `CreateFileCache` API operation response\. It's also returned in the response of the describe\-file\-caches AWS CLI command, and the `[DescribeFileCaches](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DescribeFileCaches.html)` API operation\.

   ```
   cache_dns_name@tcp:/mountname /mnt lustre defaults,relatime,flock,_netdev,x-systemd.automount,x-systemd.requires=network.service 0 0
   ```
**Warning**  
Use the `_netdev` option, used to identify network file systems, when mounting your cache automatically\. If `_netdev` is missing, your EC2 instance might stop responding\. This result is because network file systems need to be initialized after the compute instance starts its networking\.

1. Save the changes to the file\.

Your EC2 instance is now configured to mount the cache whenever it restarts\.

**Note**  
In some cases, your Amazon EC2 instance might need to start regardless of the status of your mounted cache\. In these cases, add the `nofail` option to your cache's entry in your `/etc/fstab` file\.

The fields in the line of code that you added to the `/etc/fstab` file do the following\.


| Field | Description | 
| --- | --- | 
|  `cache_dns_name@tcp:/`  |  The DNS name for your cache, which identifies it\. You can get this name from the console or programmatically from the AWS CLI or an AWS SDK\.  | 
|  `mountname`  | The mount name for the cache\. You can get this name from the console or programmatically from the AWS CLI using the describe\-file\-caches command or the AWS API or SDK using the `[DescribeFileSystems](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DescribeFileCaches.html)` operation\. | 
|  `/mnt`  |  The mount point for the cache on your EC2 instance\.  | 
|  `lustre`  |  The type of cache\.  | 
|  `mount options`  |  Mount options for the cache, presented as a comma\-separated list of the following options: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/fsx/latest/FileCacheGuide/mount-fs-auto-mount-onreboot.html)  | 
|  `x-systemd.automount,x-systemd.requires=network.service`  |  These options ensure that the auto mounter does not run until the network connectivity is online\.  | 
|  `0`  |  A value that indicates whether the cache should be backed up by `dump`\. This value should be `0`\.  | 
|  `0`  |  A value that indicates the order in which `fsck` checks caches at boot\. For caches, this value should be `0` to indicate that `fsck` should not run at startup\.  | 