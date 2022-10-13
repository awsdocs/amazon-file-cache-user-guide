# POSIX metadata support for data repositories<a name="posix-metadata-support"></a>

Amazon File Cache automatically transfers Portable Operating System Interface \(POSIX\) metadata for files, directories, and symbolic links \(symlinks\) when importing and exporting data to and from a linked Amazon S3 or NFS data repository\. When you export changes in your cache to a linked data repository, Amazon File Cache also exports POSIX metadata changes along with data changes\. Because of this metadata export, you can implement and maintain access controls between your cache and its linked data repositories\.

 Amazon File Cache imports only objects that have POSIX\-compliant object keys, such as the following\.

```
test/mydir/ 
test/
```

Amazon File Cache stores directories and symlinks as separate objects in the linked data repository\. On an S3 data repository, for example, for directories Amazon File Cache creates an S3 object with a key name that ends with a slash \("/"\), as follows:
+ The S3 object key `test/mydir/` maps to the cache directory `test/mydir`\.
+ The S3 object key `test/` maps to the cache directory `test`\.

For symlinks, Amazon File Cache uses the following Amazon S3 schema for symlinks:
+ **S3 object key** – The path to the link, relative to the Amazon File Cache mount directory
+ **S3 object data** – The target path of this symlink
+ **S3 object metadata** – The metadata for the symlink

Amazon File Cache stores POSIX metadata, including ownership, permissions, and timestamps for Amazon File Cache files, directories, and symbolic links, in S3 objects as follows:
+ `Content-Type` – The HTTP entity header used to indicate the media type of the resource for web browsers\.
+ `x-amz-meta-file-permissions` – The file type and permissions in the format `<octal file type><octal permission mask>`, consistent with `st_mode` in the [Linux stat\(2\) man page](https://man7.org/linux/man-pages/man2/lstat.2.html)\.
**Note**  
Amazon File Cache does not import or retain `setuid` information\.
+ `x-amz-meta-file-owner` – The owner user ID \(UID\) expressed as an integer\.
+ `x-amz-meta-file-group` – The group ID \(GID\) expressed as an integer\.
+ `x-amz-meta-file-atime` – The last\-accessed time in nanoseconds\. Terminate the time value with `ns`; otherwise Amazon File Cache interprets the value as milliseconds\.
+ `x-amz-meta-file-mtime` – The last\-modified time in nanoseconds\. Terminate the time value with `ns`; otherwise, Amazon File Cache interprets the value as milliseconds\.
+ `x-amz-meta-user-agent` – The user agent, ignored during Amazon File Cache import\. During export, Amazon File Cache sets this value to `aws-fsx-lustre`\.

The default POSIX permission that Amazon File Cache assigns to a file is 755\. This permission allows read and execute access for all users and write access for the owner of the file\.

**Note**  
Amazon File Cache doesn't retain any user\-defined custom metadata on S3 objects\.