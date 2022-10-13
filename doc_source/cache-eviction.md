# Cache eviction<a name="cache-eviction"></a>

Files can be evicted \(released\) from the cache to free up space for new files\. There are two methods to evict files:
+ Automatic cache eviction evicts files automatically, based on the cache's eviction policy\.
+ You manually use HSM commands to release files\.

**Important**  
Both methods only evict files that are in the archived state\. You must first export the files to your linked data repository using HSM commands, as described in [Exporting files using HSM commands](exporting-files-hsm.md)\.

## Automatic cache eviction<a name="auto-cache-eviction"></a>

File Cache automatically manages the cache storage capacity by releasing the less recently used files on your cache when the cache begins to fill up\. Automatic cache eviction is enabled by default when you create a cache using the File Cache console, AWS CLI, or the AWS API\.

## Releasing files using HSM commands<a name="hsm-release-files"></a>

If you want to create storage space, you can release files from your cache\. Releasing a file retains the file listing and metadata, but removes the local copy of that file's contents\. You cannot release a file if it's in use or if it has not been exported to a linked data repository\. You can release individual files from your cache using the following commands:
+ To release one or more files from your cache if you are the file owner:

  ```
  lfs hsm_release file1 file2 ...
  ```
+ To release one or more files from your cache if you are not the file owner:

  ```
  sudo lfs hsm_release file1 file2 ...
  ```