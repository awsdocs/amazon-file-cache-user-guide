# Encrypting data in transit<a name="encryption-in-transit"></a>

Encryption of data in transit is automatically enabled when you access an Amazon File Cache cache from compute instances that support encryption in transit\. To learn which EC2 instances support encryption in transit, see [Encryption in Transit](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/data-protection.html#encryption-transit) in the *Amazon EC2 User Guide for Linux Instances*\.

Amazon File Cache encrypts traffic between the cache and your S3 data repositories using HTTPS \(TLS\)\. Amazon File Cache does not encrypt traffic between the cache and your NFSv3 data repositories, as the NFSv3 protocol does not encrypt data at the protocol level\. For encryption in transit between your on\-premises file systems and Amazon File Cache, you can use a virtual private network \(VPN\) to ensure encrypted data transfers between your VPC and your on\-premises network\. If you're using AWS Direct Connect, you can use AWS VPN to combine one or more AWS Direct Connect dedicated network connections with AWS VPN\. Additionally, you can use [MAC Security](https://docs.aws.amazon.com/directconnect/latest/UserGuide/MACsec.html) \(MACsec\) to encrypt your data from your corporate data center to the AWS Direct Connect location\. For more information, see [Encryption in AWS Direct Connect](https://docs.aws.amazon.com/directconnect/latest/UserGuide/encryption-in-transit.html)\.

You can explicitly prohibit some or all users in your organization from creating Amazon File Cache resources linked to NFSv3 file systems using the `fsx:NfsDataRepositoryEncryptionInTransitEnabled` and `fsx:NfsDataRepositoryAuthenticationEnabled` context keys to control access to the `CreateFileCache` API action\. The following is an example of an AWS Organizations Service control policy \(SCP\) that prohibits creation of a Amazon File Cache resource linked to an NFSv3 data repository:

```
{
    "Version": "2012-10-17",
    "Statement":
    [
        {
            "Effect": "Deny",
            "Action": "fsx:*",
            "Resource": "*",
            "Condition":
            {
                "Bool":
                {
                    "fsx:NfsDataRepositoryEncryptionInTransitEnabled": "false"
                }
            }
        },
        {
            "Effect": "Deny",
            "Action": "fsx:*",
            "Resource": "*",
            "Condition":
            {
                "Bool":
                {
                    "fsx:NfsDataRepositoryAuthenticationEnabled": "false"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

For more information about IAM condition keys, see [Policy condition keys for File Cache](security_iam_service-with-iam.md#security_iam_service-with-iam-id-based-policies-conditionkeys)\.