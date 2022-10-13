# Data encryption in Amazon File Cache<a name="encryption"></a>

Amazon File Cache supports two forms of data encryption for caches, encryption of data at rest and encryption in transit\. Encryption of data at rest is automatically enabled when creating an Amazon File Cache cache\. Encryption of data in transit is automatically enabled when you access an Amazon File Cache cache from [Amazon EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/data-protection.html#encryption-transit) that support this feature\.

## When to use encryption<a name="whenencrypt"></a>

If your organization is subject to corporate or regulatory policies that require encryption of data and metadata at rest, we recommend creating an encrypted cache and mounting your cache using encryption of data in transit\.

**Topics**
+ [When to use encryption](#whenencrypt)
+ [Encrypting data at rest](encryption-at-rest.md)
+ [Encrypting data in transit](encryption-in-transit.md)
+ [How Amazon File Cache uses AWS KMS](FileCacheKMS.md)