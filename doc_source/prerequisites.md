# Prerequisites<a name="prerequisites"></a>

To perform this getting started exercise, you need the following:
+ An AWS account with the permissions necessary to create an Amazon File Cache and an Amazon EC2 instance\. For more information, see [Setting up](setting-up.md)\.
+ An Amazon EC2 instance running a supported Linux release in your virtual private cloud \(VPC\) based on the Amazon VPC service\. You will install the Lustre client on this EC2 instance, and then mount your cache on the EC2 instance\. The Lustre client supports  CentOS and Red Hat Enterprise Linux 7\.9 and 8\.4 through 8\.6, Rocky Linux 8\.4 through 8\.6, and Ubuntu 18\.04, 20\.04, and 22\.04\. For this getting started exercise, we will use Ubuntu 22\.04\.

  When creating your Amazon EC2 instance for this getting started exercise, keep the following in mind:
  + We recommend that you create your instance in your default VPC\.
  + We recommend that you use the default security group when creating your EC2 instance\.
+ An Amazon S3 bucket storing the data for your workload to process\. The S3 bucket will be the linked data repository for your cache\.