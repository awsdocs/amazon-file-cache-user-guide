# Encrypting data at rest<a name="encryption-at-rest"></a>

Encryption of data at rest is automatically enabled when you create an Amazon File Cache file system through the AWS Management Console, the AWS CLI, or programmatically through the AWS API or one of the AWS SDKs\. Your organization might require the encryption of all data that meets a specific classification or is associated with a particular application, workload, or environment\. You can specify the AWS Key Management Service \(AWS KMS\) key to use for encrypting the data when you create an Amazon File Cache resource\. For more information about creating a cache encrypted at rest using the console, see [Create Your Amazon File Cache File System](getting-started-step1.md)\.

**Note**  
The AWS key management infrastructure uses Federal Information Processing Standards \(FIPS\) 140\-2 approved cryptographic algorithms\. The infrastructure is consistent with National Institute of Standards and Technology \(NIST\) 800\-57 recommendations\.

For more information, see [How Amazon File Cache uses AWS KMS](FileCacheKMS.md)\.

## How encryption at rest works<a name="howencrypt"></a>

Data and metadata are automatically encrypted before being written to the cache\. When you attach the encrypted volume to an instance, Amazon EC2 sends a `CreateGrant` request to AWS KMS, so that it can decrypt the data key\. Amazon EC2 uses the plaintext data key in hypervisor memory to encrypt disk I/O to the volume\. The plaintext data key persists in memory as long as the volume is attached to the instance\.Similarly, as data and metadata are read, they are automatically decrypted before being presented to the application\. These processes are handled transparently by Amazon File Cache, so you don't have to modify your applications\.

Amazon File Cache uses industry\-standard AES\-256 encryption algorithm to encrypt file system data at rest\. For more information, see [Cryptography Basics](https://docs.aws.amazon.com/kms/latest/developerguide/crypto-intro.html) in the *AWS Key Management Service Developer Guide*\.