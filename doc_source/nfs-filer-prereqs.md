# Prerequisites for linking to on\-premises NFS data repositories<a name="nfs-filer-prereqs"></a>

Before you can link your cache to an on\-premises NFS data store, make sure that your resources and configurations meet the following requirements:
+ Your on\-premises NFS file system must support NFSv3\.
+ If you're using a domain name to link your NFS file system to File Cache, you must provide the IP address of a DNS server that Amazon File Cache can use to resolve the domain name of the on\-premises NFSv3 file system\. The DNS server can be located in the VPC where you plan to create the cache, or it can be on your on\-premises network accessible from your VPC\.
+ The DNS server and on premises NFSv3 file system must use private IP addresses, as specified in RFC 1918:
  + 10\.0\.0\.0\-10\.255\.255\.255 \(10/8 prefix\)
  + 172\.16\.0\.0\-172\.31\.255\.255 \(172\.16/12 prefix\)
  + 192\.168\.0\.0\-192\.168\.255\.255 \(192\.168/16 prefix\)
+ You must establish an AWS Direct Connect or VPN connection between your on\-premises network and the Amazon VPC where your cache is located\. For more information on AWS Direct Connect, see the *AWS Direct Connect User Guide*\. For more information on setting up a VPN connection, see the *Amazon VPC User Guide*\.
**Important**  
Use an AWS Site\-to\-Site VPN \(AWS VPN\) connection if you want to encrypt data as it transits between your Amazon VPC and your on\-premises network\. For more information, see [What is AWS Site\-to\-Site VPN?](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html)
+ Your on\-premises firewall must allow traffic between IP addresses in your Amazon VPC subnet IP CIDR and the IP addresses of the DNS server and the on\-premises NFSv3 file system\.
+ Your on\-premises NFSv3 file system is configured to allow access to IP addresses on the Amazon VPC where the cache is located\.
+ The Amazon VPC Security Group used for your cache must be configured to allow outbound traffic to the IP addresses of the DNS server and on\-premises NFSv3 file system\. Make sure to add outbound rules to allow port 53 for both UDP and TCP for DNS traffic, and to allow the TCP ports used by the on\-premises NFSv3 file system for NFS\. For more information, see [Controlling access using inbound and outbound rules](limit-access-security-groups.md#inbound-outbound-rules)\. 
+ While Amazon File Cache supports NFSv3 file systems with most NFSv3 export policies including `root_squash`, you must not use the NFS export option `all_squash`\. This configuration is required so that Amazon File Cache has the necessary permissions to read and write files owned by all users on your NFSv3 file system\.