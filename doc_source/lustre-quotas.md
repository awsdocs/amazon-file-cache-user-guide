# Storage quotas<a name="lustre-quotas"></a>

You can create storage quotas for users and groups on Amazon File Cache resources\. With storage quotas, you can limit the amount of disk space and the number of files that a user or a group can consume\. Storage quotas automatically track user\- and group\-level usage so you can monitor consumption whether or not you choose to set storage limits\.

Amazon File Cache enforces quotas and prevents users who have exceeded them from writing to the storage space\. When users exceed their quotas, they must delete enough files to get under the quota limits so that they can write to the cache again\.

Storage quotas are enforced only when your users write data to the cache directly\. Amazon File Cache acts as a user with root access when loading data into the cache from a data repository and thus bypasses quota enforcement\. Once data has been imported, it counts towards the user or group based on the ownership set in the data repository, which can cause users or groups to exceed their block limits\. If this occurs, they'll need to free up files to be able to write to the cache again\.

**Topics**
+ [Quota enforcement](#quotas-enforcement)
+ [Types of quotas](#quota-types)
+ [Quota limits and grace periods](#quota-limits)
+ [Setting and viewing quotas](#setting-quotas)
+ [Quotas and linked data repositories](#quotas-s3)

## Quota enforcement<a name="quotas-enforcement"></a>

User and group quota enforcement is automatically enabled on all caches\. You cannot disable quota enforcement\.

## Types of quotas<a name="quota-types"></a>

System administrators with AWS account root user credentials can create the following types of quotas:
+ A *user quota* applies to an individual user\. A user quota for a specific user can be different from the quotas of other users\.
+ A *group quota* applies to all users who are members of a specific group\.
+ A *block quota* limits the amount of disk space that a user or group can consume\. You configure the storage size in kilobytes\.
+ An *inode quota* limits the number of files or directories that a user or group can create\. You configure the maximum number of inodes as an integer\.

**Note**  
Default quotas and project quotas are not supported\.

If you set quotas for a particular user and a group, and the user is a member of that group, the user's data usage applies to both quotas\. It is also limited by both quotas\. If either quota limit is reached, the user is blocked from writing to the cache\.

**Note**  
Quotas set for the root user are not enforced\. Similarly, writing data as the root user using the `sudo` command bypasses enforcement of the quota\.

## Quota limits and grace periods<a name="quota-limits"></a>

Amazon File Cache enforces user and group quotas as a hard limit or as a soft limit with a configurable grace period\.

The hard limit is the absolute limit\. If users exceed their hard limit, a block or inode allocation fails with a Disk quota exceeded message\. Users who have reached their quota hard limit must delete enough files or directories to get under the quota limit before they can write to the cache again\. When a grace period is set, users can exceed the soft limit within the grace period if under the hard limit\.

For soft limits, you configure a grace period in seconds\. The soft limit must be smaller than the hard limit\.

You can set different grace periods for inode and block quotas\. You can also set different grace periods for a user quota and a group quota\. When user and group quotas have different grace periods, the soft limit transforms to a hard limit after the grace period of either user or group quota elapses\.

When users exceed a soft limit, Amazon File Cache allows them to continue exceeding their quota until the grace period has elapsed or until the hard limit is reached\. After the grace period ends, the soft limit converts to a hard limit, and users are blocked from any further write operations until their storage usage returns below the defined block quota or inode quota limits\. Users don't receive a notification or warning when the grace period begins\.

## Setting and viewing quotas<a name="setting-quotas"></a>

You set storage quotas using Lustre file system `lfs` commands in your Linux terminal\. The `lfs setquota` command sets quota limits, and the `lfs quota` command displays quota information\.

For more information about Lustre quota commands, see the *Lustre Operations Manual* on the [Lustre documentation website](http://lustre.org/documentation/)\.

### Setting user and group quotas<a name="setting-user-quotas"></a>

The syntax of the `setquota` command for setting user or group quotas is as follows\.

```
lfs setquota {-u|--user|-g|--group} username|groupname
             [-b block_softlimit] [-B block_hardlimit]
             [-i inode_softlimit] [-I inode_hardlimit]
             /mount_point
```

Where:
+ `-u` or `--user` specifies a user to set a quota for\.
+ `-g` or `--group` specifies a group to set a quota for\.
+ `-b` sets a block quota with a soft limit\. `-B` sets a block quota with a hard limit\. Both *block\_softlimit* and *block\_hardlimit* are expressed in kilobytes, and the minimum value is 1024 KB\.
+ `-i` sets an inode quota with a soft limit\. `-I` sets an inode quota with a hard limit\. Both *inode\_softlimit* and *inode\_hardlimit* are expressed in number of inodes, and the minimum value is 1024 inodes\.
+ *mount\_point* is the directory that the cache was mounted on\.

The following command sets a 5,000 KB soft block limit, an 8,000 KB hard block limit, a 2,000 soft inode limit, and a 3,000 hard inode limit quota for `user1` on the cache mounted to `/mnt`\.

```
sudo lfs setquota -u user1 -b 5000 -B 8000 -i 2000 -I 3000 /mnt
```

The following command sets a 100,000 KB hard block limit for the group named `group1` on the cache mounted to `/mnt`\.

```
sudo lfs setquota -g group1 -B 100000 /mnt
```

### Setting grace periods<a name="setting-grace"></a>

The default grace period is one week\. You can adjust the default grace period for users and groups, using the following syntax\.

```
lfs setquota -t {-u|-g}
             [-b block_grace]
             [-i inode_grace]
             /mount_point
```

Where:
+ `-t` indicates that a grace time period will be set\.
+ `-u` sets a grace period for all users\.
+ `-g` sets a grace period for all groups\.
+ `-b` sets a grace period for block quotas\. `-i` sets a grace period for inode quotas\. Both *block\_grace* and *inode\_grace* are expressed in integer seconds or in the `XXwXXdXXhXXmXXs` format\.
+ *mount\_point* is the directory that the cache was mounted on\.

The following command sets grace periods of 1,000 seconds for user block quotas and 1 week and 4 days for user inode quotas\.

```
sudo lfs setquota -t -u -b 1000 -i 1w4d /mnt
```

### Viewing quotas<a name="viewing-quotas"></a>

The `quota` command displays information about user quotas, group quotas, and grace periods\.


| View quota command | Quota information displayed | 
| --- | --- | 
|  `lfs quota /mount_point`  |  General quota information \(disk usage and limits\) for the user running the command and the user's primary group\.  | 
|  `lfs quota -u username /mount_point`  |  General quota information for a specific user\. Users with AWS account root user credentials can run this command for any user, but non\-root users can't run this command to get quota information about other users\.  | 
|  `lfs quota -u username -v /mount_point`  |  General quota information for a specific user and detailed quota statistics for each object storage target \(OST\) and metadata target \(MDT\)\. Users with AWS account root user credentials can run this command for any user, but non\-root users can't run this command to get quota information about other users\.  | 
|  `lfs quota -g groupname /mount_point`  |  General quota information for a specific group\.  | 
| `lfs quota -t -u /mount_point` | Block and inode grace times for user quotas\. | 
| `lfs quota -t -g /mount_point` | Block and inode grace times for group quotas\. | 

## Quotas and linked data repositories<a name="quotas-s3"></a>



You can link your cache to an Amazon S3 or NFS data repository\. For more information, see [Linking your cache to a data repository](create-linked-data-repo.md)\.

You can optionally choose a specific folder or prefix within a linked data repository as an import path to your cache\. Only data loaded into the cache from the linked data repository is applied towards the quota\. The data of the entire repository is not counted against the quota limits\.

File metadata in a linked data repository are imported into a folder with a structure matching the imported folder from the repository\. These files count towards the inode quotas of the users and groups who own the files\.

When a user performs an `hsm_restore` or lazy loads a file, the file's full size counts towards the block quota associated with the owner of the file\. For example, if user A lazy loads a file that is owned by user B, the amount of storage and inode usage counts towards user B's quota\. Similarly, when a user uses the `hsm_release` command on a file, the data is freed up from the block quotas of the user or group who owns the file\.

Amazon File Cache acts as a user with root access when loading data into the cache from a data repository and thus bypasses quota enforcement\.