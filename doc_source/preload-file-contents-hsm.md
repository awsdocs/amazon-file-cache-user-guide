# Preloading files into your cache<a name="preload-file-contents-hsm"></a>

If the data you're accessing doesn't already exist in the cache, Amazon File Cache copies the data from your Amazon S3 or NFS data repository into the cache in line with file access\. Because of this approach, the initial read or write to a file incurs a small amount of latency\. If your application is sensitive to this latency, and you know which files or directories your application needs to access, you can optionally preload contents of individual files or directories\. You do so using the `hsm_restore` command, as follows\.

You can use the `hsm_action` command \(issued with the `lfs` user utility\) to verify that the file's contents have finished loading into the cache\. A return value of `NOOP` indicates that the file has successfully been loaded\. Run the following commands from a compute instance with the cache mounted\. Replace *path/to/file* with the path of the file you're preloading into your cache\.

```
sudo lfs hsm_restore path/to/file
sudo lfs hsm_action path/to/file
```

You can preload your whole cache or an entire directory within your cache by using the following commands\. \(The trailing ampersand makes a command run as a background process\.\) If you request the preloading of multiple files simultaneously, Amazon File Cache loads your files from your linked data repository in parallel\.

```
nohup find local/directory -type f -print0 | xargs -0 -n 1 sudo lfs hsm_restore &
```

**Note**  
If your linked data repository is larger than your cache, you can load only as much actual file data as will fit into the cache's remaining storage space\. You'll receive an error if you attempt to access file data when there is no more storage left on the cache\.