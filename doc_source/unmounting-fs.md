# Unmounting caches<a name="unmounting-fs"></a>

Before you delete a cache, we recommend that you unmount it from every Amazon EC2 instance that it's connected to\. You can unmount a cache on your Amazon EC2 instance by running the `umount` command on the instance itself\. You can't unmount a cache through the AWS CLI, the AWS Management Console, or through any of the AWS SDKs\. To unmount a cache connected to an Amazon EC2 instance running Linux, use the `umount` command as follows:

```
umount /mnt 
```

We recommend that you do not specify any other `umount` options\. Avoid setting any other `umount` options that are different from the defaults\.

You can verify that your cache has been unmounted by running the `df` command\. This command displays the disk usage statistics for the file caches currently mounted on your Linux\-based Amazon EC2 instance\. If the cache that you want to unmount isn’t listed in the `df` command output, this means that the cache is unmounted\.

**Example – Identify the mount status of an Amazon File Cache resource and unmount it**  

```
$ df -T
Filesystem Type 1K-blocks Used Available Use% Mounted on 
cache_id.fsx.aws-region.amazonaws.com@tcp:/mountname /mnt 3547708416 61440 3547622400 1% /mnt
      /dev/sda1 ext4 8123812 1138920 6884644 15% /
```

```
$ umount /mnt
```

```
$ df -T 
```

```
Filesystem Type 1K-blocks Used Available Use% Mounted on 
/dev/sda1 ext4 8123812 1138920 6884644 15% /
```