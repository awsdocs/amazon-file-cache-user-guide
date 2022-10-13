# Importing files from your data repository<a name="importing-files"></a>

When you create a Amazon File Cache resource, you can create a data repository association \(DRA\) to link your cache to an Amazon S3 or NFS data repository\. Amazon File Cache transparently copies the content of a file from your repository and loads it into the cache, if it doesn't already exist, when your application accesses the file\.

You can also preload your whole cache or an entire directory within your cache; for more information, see [Preloading files into your cache](preload-file-contents-hsm.md)\. This data movement is managed by Amazon File Cache and occurs transparently to your applications\. Subsequent reads of these files are served directly out of Amazon File Cache with consistent sub\-millisecond latencies\. If you request the preloading of multiple ﬁles simultaneously, Amazon File Cache loads your ﬁles from your linked data repository in parallel\. For more information, see [Lazy load](mdll-lazy-load.md)\.

Amazon File Cache *only* imports objects that have POSIX\-compliant object keys, such as these:

```
test/mydir/ 
test/
```

**Note**  
For a linked S3 bucket, Amazon File Cache doesn't support importing metadata for symbolic links \(symlinks\) from S3 Glacier Flexible Retrieval and S3 Glacier Deep Archive storage classes\. Metadata for S3 Glacier Flexible Retrieval objects that are not symlinks can be imported \(that is, an inode is created on the cache with the correct metadata\)\. However, to retrieve the data, you must restore the S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive object first and then use an `hsm_restore` command to import the object\. Importing file data directly from Amazon S3 objects in the S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive storage class into Amazon File Cache is not supported\.