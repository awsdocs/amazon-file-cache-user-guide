# Linking your cache to a data repository<a name="create-linked-data-repo"></a>

You can link your cache to data repositories in Amazon S3 or on NFS \(Network File System\) file systems that support the NFSv3 protocol\. The NFS file systems can be on\-premises or in\-cloud file systems\. You create the links when you create your cache\.

A link between a directory on your cache and an S3 or NFS data repository is called a *data repository association \(DRA\)*\. You can create a maximum of 8 data repository associations on a Amazon File Cache resource\.  Each DRA must have a unique Amazon File Cache directory and an S3 bucket or NFS file system associated with it\.

**Note**  
An Amazon File Cache resource can link to either S3 or NFS data repositories, but not to both types at the same time\. All the DRAs on the cache must link to the same data repository type \(S3 or NFS\)\.

By default, File Cache automatically loads data into the cache when it’s accessed for the first time \(lazy load\)\. You can optionally pre\-load data into the cache before starting your workload\.

**Note**  
You should not modify the same file on both the data repository and the cache at the same time, otherwise the behavior is undefined\.