# Accessing caches<a name="accessing-caches"></a>

In the following topics, you can learn how to access your cache on a Linux instance\. In addition, you can find how to use the file `fstab` to automatically remount your cache after any system restarts\.

Before you can mount a cache, you must create, configure, and launch your related AWS resources\. For detailed instructions, see [Getting started with Amazon File Cache](getting-started.md)\. Next, you can install and configure the Lustre client on your compute instance\.

**Topics**
+ [Installing the Lustre client](install-lustre-client.md)
+ [Mounting from an Amazon Elastic Compute Cloud instance](mounting-ec2-instance.md)
+ [Mounting from Amazon Elastic Container Service](mounting-ecs.md)
+ [Mounting caches from on\-premises or a peered Amazon VPC](mounting-on-premises.md)
+ [Mounting your cache automatically](mount-fs-auto-mount-onreboot.md)
+ [Mounting specific filesets](mounting-from-fileset.md)
+ [Unmounting caches](unmounting-fs.md)
+ [Working with Amazon EC2 Spot Instances](working-with-ec2-spot-instances.md)