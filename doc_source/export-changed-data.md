# Exporting changes to the data repository<a name="export-changed-data"></a>

You can export data and metadata changes, including POSIX metadata, from Amazon File Cache to a linked Amazon S3 or NFS data repository\. Associated POSIX metadata includes ownership, permissions, and timestamps\. To export changes from the cache, you use HSM commands\. When you export a file or directory using HSM commands, your cache exports only data files and metadata that were created or modified since the last export, For more information, see [Exporting files using HSM commands](exporting-files-hsm.md)\.

**Important**  
To ensure that Amazon File Cache can export your data to your linked data repository, it must be stored in a UTF\-8 compatible format\.

**Topics**
+ [Exporting files using HSM commands](exporting-files-hsm.md)