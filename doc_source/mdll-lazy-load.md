# Lazy load<a name="mdll-lazy-load"></a>

When you access data on a linked Amazon S3 or NFS data repository using the cache, Amazon File Cache automatically loads the metadata \(the name, ownership, timestamps, and permissions\) and file contents if they are not already present in the cache\. The data in your data repositories appears as files and directories in the cache\. 

Lazy load is triggered when you are in a DRA directory and you read or write data or metadata to a file\. Amazon File Cache loads data into the cache from the linked data repositories if it's not already available\. For example, lazy load is triggered when you open a file, stat a file, or make metadata updates to the file\.

You can also trigger lazy load by using the `ls` command to list the contents of a DRA directory\. If you're at the root of a directory hierarchy that includes several DRA directories, the `ls` command will use lazy load on all the DRA directories in the hierarchy\. For example, if you’re at `/` in the directory tree, and your four DRAs are `/a`, `/b`, `/c`, and `/d`, then running a recursive `ls` command populates metadata for all DRAs\. To run a recursive `ls` command, use the `-R` option, as in these two examples:

```
ls -R
ls -R /tmp/dir1
```

When you use the `ls` or `stat` commands, File Cache only loads file and directory metadata for requested files; no file content will be downloaded\. The data from a file in the data repository is actually downloaded to your cache when the file is read\.