# Amazon File Cache File Cache User Guide

-----
*****Copyright &copy; Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in
connection with any product or service that is not Amazon's,
in any manner that is likely to cause confusion among customers,
or in any manner that disparages or discredits Amazon. All other
trademarks not owned by Amazon are the property of their respective
owners, who may or may not be affiliated with, connected to, or
sponsored by Amazon.

-----
## Contents
+ [What is Amazon File Cache?](what-is.md)
+ [Setting up](setting-up.md)
+ [Getting started with Amazon File Cache](getting-started.md)
   + [Prerequisites](prerequisites.md)
   + [Step 1: Create your cache](getting-started-step1.md)
   + [Step 2: Install and configure the Lustre client on your instance before mounting your cache](getting-started-step2.md)
   + [Step 3: Run your analysis](getting-started-step3.md)
   + [Step 4: Clean up resources](getting-started-step4.md)
+ [Using data repositories with Amazon File Cache](using-data-repositories.md)
   + [Overview of data repositories](overview-data-repo.md)
      + [POSIX metadata support for data repositories](posix-metadata-support.md)
      + [Walkthrough: Attaching POSIX permissions when uploading objects into an Amazon S3 bucket](attach-s3-posix-permissions.md)
      + [Prerequisites for linking to on-premises NFS data repositories](nfs-filer-prereqs.md)
   + [Linking your cache to a data repository](create-linked-data-repo.md)
      + [Creating a link to a data repository](create-linked-repo.md)
      + [Working with server-side encrypted Amazon S3 buckets](s3-server-side-encryption-support.md)
   + [Importing files from your data repository](importing-files.md)
      + [Lazy load](mdll-lazy-load.md)
      + [Preloading files into your cache](preload-file-contents-hsm.md)
   + [Exporting changes to the data repository](export-changed-data.md)
      + [Exporting files using HSM commands](exporting-files-hsm.md)
   + [Cache eviction](cache-eviction.md)
+ [Amazon File Cache performance](performance.md)
+ [Accessing caches](accessing-caches.md)
   + [Installing the Lustre client](install-lustre-client.md)
   + [Mounting from an Amazon Elastic Compute Cloud instance](mounting-ec2-instance.md)
   + [Mounting from Amazon Elastic Container Service](mounting-ecs.md)
   + [Mounting caches from on-premises or a peered Amazon VPC](mounting-on-premises.md)
   + [Mounting your cache automatically](mount-fs-auto-mount-onreboot.md)
   + [Mounting specific filesets](mounting-from-fileset.md)
   + [Unmounting caches](unmounting-fs.md)
   + [Working with Amazon EC2 Spot Instances](working-with-ec2-spot-instances.md)
+ [Managing Amazon File Cache resources](managing-resources.md)
   + [Managing caches](managing-caches.md)
   + [Storage quotas](lustre-quotas.md)
   + [Tag your Amazon File Cache resources](tag-resources.md)
   + [Amazon File Cache maintenance windows](maintenance-windows.md)
+ [Monitoring Amazon File Cache](monitoring_overview.md)
   + [Monitoring with Amazon CloudWatch](monitoring-cloudwatch.md)
      + [Front-end I/O metrics](frontend-io-metrics.md)
      + [Back-end I/O metrics](backend-io-metrics.md)
      + [Cache utilization metrics](utilization-metrics.md)
      + [How to use Amazon File Cache metrics](how_to_use_metrics.md)
      + [Accessing CloudWatch metrics](accessingmetrics.md)
      + [Creating CloudWatch alarms to monitor Amazon File Cache](creating_alarms.md)
   + [Logging Amazon File Cache API calls with AWS CloudTrail](logging-using-cloudtrail.md)
+ [Security in Amazon File Cache](security.md)
   + [Data protection in Amazon File Cache](data-protection.md)
      + [Data encryption in Amazon File Cache](encryption.md)
         + [Encrypting data at rest](encryption-at-rest.md)
         + [Encrypting data in transit](encryption-in-transit.md)
         + [How Amazon File Cache uses AWS KMS](FileCacheKMS.md)
   + [Internetwork traffic privacy](internetwork-privacy.md)
   + [Identity and Access Management for Amazon File Cache](security-iam.md)
      + [How Amazon File Cache works with IAM](security_iam_service-with-iam.md)
      + [Identity-based policy examples for Amazon File Cache](security_iam_id-based-policy-examples.md)
      + [AWS managed policies for Amazon File Cache](security-iam-awsmanpol.md)
      + [Troubleshooting Amazon File Cache identity and access](security_iam_troubleshoot.md)
      + [Using tags with Amazon File Cache](using-tags.md)
      + [Using service-linked roles for Amazon File Cache](using-service-linked-roles.md)
   + [Cache access control with Amazon VPC](limit-access-security-groups.md)
   + [Amazon VPC Network ACLs](limit-access-acl.md)
   + [Compliance Validation for Amazon File Cache](fsx-lustre-compliance.md)
   + [Amazon File Cache and interface VPC endpoints (AWS PrivateLink)](vpc-endpoints.md)
+ [Quotas](limits.md)
+ [Document history](doc-history.md)