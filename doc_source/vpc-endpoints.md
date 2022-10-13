# Amazon File Cache and interface VPC endpoints \(AWS PrivateLink\)<a name="vpc-endpoints"></a>

You can improve the security posture of your VPC by configuring Amazon File Cache to use an interface VPC endpoint\. Interface VPC endpoints are powered by [AWS PrivateLink](http://aws.amazon.com/privatelink), a technology that enables you to privately access AWS APIs without an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. Instances in your VPC don't need public IP addresses to communicate with AWS APIs\. Traffic between your VPC and Amazon FSx does not leave the AWS network\.

Each interface VPC endpoint is represented by one or more elastic network interfaces in your subnets\. A network interface provides a private IP address that serves as an entry point for traffic to the AWS API\.



## Considerations for Amazon File Cache interface VPC endpoints<a name="privatelink-considerations"></a>

Before you set up an interface VPC endpoint for Amazon File Cache, be sure to review [ Interface VPC endpoint properties and limitations](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface.html#vpce-interface-limitations) in the *Amazon VPC User Guide*\.

You can call any of the Amazon File Cache API operations from your VPC\. For example, you can create an FSx for Lustre file system by calling the CreateFileSystem API from within your VPC\. For the full list of Amazon File Cache APIs, see [Actions](https://docs.aws.amazon.com/fsx/latest/APIReference/API_Operations.html) in the Amazon FSx API Reference\.

### VPC peering considerations<a name="privatelink-vpc-peering"></a>

You can connect other VPCs to the VPC with interface VPC endpoints using VPC peering\. VPC peering is a networking connection between two VPCs\. You can establish a VPC peering connection between your own two VPCs, or with a VPC in another AWS account\. The VPCs can also be in two different AWS Regions\.

Traffic between peered VPCs stays on the AWS network and does not traverse the public internet\. Once VPCs are peered, resources like Amazon Elastic Compute Cloud \(Amazon EC2\) instances in both VPCs can access the Amazon FSx API through interface VPC endpoints created in the one of the VPCs\.

## Creating an interface VPC endpoint<a name="create-vpce"></a>

You can create a VPC endpoint for the Amazon File Cache API using either the Amazon VPC console or the AWS Command Line Interface \(AWS CLI\)\. For more information, see [ Creating an interface VPC endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) in the *Amazon VPC User Guide*\.

For a complete list of endpoints for Amazon FSx services, see [Amazon FSx endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/fsxn.html) in the *Amazon Web Services General Reference*\. 

To create an interface VPC endpoint for Amazon File Cache, use one of the following:
+ `com.amazonaws.region.fsx` – Creates an endpoint for Amazon File Cache API operations\.
+ **`com.amazonaws.region.fsx-fips`** – Creates an endpoint for the Amazon File Cache API that complies with [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

To use the private DNS option, you must set the `enableDnsHostnames` and `enableDnsSupport` attributes of your VPC\. For more information, see [ Viewing and updating DNS support for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html#vpc-dns-updating) in the *Amazon VPC User Guide*\.

If you enable private DNS for the endpoint, you can make API requests to Amazon FSx with the VPC endpoint using its default DNS name for the AWS Region, for example `fsx.us-east-1.amazonaws.com`\. 

For more information, see [ Accessing a service through an interface VPC endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#access-service-though-endpoint) in the *Amazon VPC User Guide*\.

## Creating a VPC endpoint policy<a name="create-vpce-policy-fsx"></a>

To further control access to the Amazon File Cache API, you can optionally attach an AWS Identity and Access Management \(IAM\) policy to your VPC endpoint\. The policy specifies the following:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources on which actions can be performed\. 

For more information, see [Controlling access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\.