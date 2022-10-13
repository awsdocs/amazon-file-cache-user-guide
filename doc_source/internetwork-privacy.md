# Internetwork traffic privacy<a name="internetwork-privacy"></a>

This topic describes how File Cache secures connections from the service to other locations\.

## Traffic between Amazon File Cache and on\-premises clients<a name="inter-network-traffic-privacy-on-prem"></a>

You have two connectivity options between your private network and AWS:
+ An AWS Site\-to\-Site VPN connection\. For more information, see [What is AWS Site\-to\-Site VPN?](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html)
+ An AWS Direct Connect connection\. For more information, see [What is AWS Direct Connect?](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html)

You can access Amazon File Cache over the network to reach AWS\-published API operations for performing administrative tasks and Lustre ports to interact with the cache\.

Access to File Cache via the network is through AWS\-published APIs\. Clients must support Transport Layer Security \(TLS\) 1\.0\. We recommend TLS 1\.2 or higher\. Clients must also support cipher suites with Perfect Forward Secrecy \(PFS\), such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Diffie\-Hellman Ephemeral \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\. Additionally, requests must be signed by using an access key ID and a secret access key that is associated with an IAM principal\. Or you can use the [AWS Security Token Service \(STS\)](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) to generate temporary security credentials to sign requests\.

## API traffic between AWS resources in the same Region<a name="inter-network-traffic-privacy-within-region"></a>

An Amazon Virtual Private Cloud \(Amazon VPC\) endpoint for Amazon File Cache is a logical entity within a VPC that allows connectivity only to Amazon File Cache\. The Amazon VPC routes API requests to Amazon File Cache and routes responses back to the VPC\. For more information, see [VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) in the *Amazon VPC User Guide*\. 