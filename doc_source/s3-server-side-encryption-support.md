# Working with server\-side encrypted Amazon S3 buckets<a name="s3-server-side-encryption-support"></a>

Amazon File Cache supports Amazon S3 buckets that use server\-side encryption with S3\-managed keys \(SSE\-S3\), and with AWS KMS keys stored in AWS Key Management Service \(SSE\-KMS\)\. 

If you want Amazon File Cache to encrypt data when writing to your S3 bucket, you need to set the default encryption on your S3 bucket to either SSE\-S3 or SSE\-KMS\. For more information, see [Enabling Amazon S3 default bucket encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/default-bucket-encryption.html) in the *Amazon S3 User Guide*\. When writing files to your S3 bucket, Amazon File Cache follows the default encryption policy of your S3 bucket\.

By default, Amazon File Cache supports S3 buckets encrypted using SSE\-S3\. If you want to link your cache to an S3 bucket encrypted using SSE\-KMS encryption, you need to add a statement to your customer managed key policy that allows Amazon File Cache to encrypt and decrypt objects in your S3 bucket using your KMS key\.

The following statement allows a specific Amazon File Cache to encrypt and decrypt objects for a specific S3 bucket, *bucket\_name*\.

```
{
    "Sid": "Allow access through S3 for the FSx SLR to use the KMS key on the objects in the given S3 bucket",
    "Effect": "Allow",
    "Principal": {
        "AWS": "arn:aws:iam::aws_account_id:role/aws-service-role/s3.data-source.lustre.fsx.amazonaws.com/AWSServiceRoleForFSxS3Access_file_cache_id"
    },
    "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
    ],
    "Resource": "*",
    "Condition": {
        "StringEquals": {
            "kms:CallerAccount": "aws_account_id",
            "kms:ViaService": "s3.bucket-region.amazonaws.com"
        },
        "StringLike": {
            "kms:EncryptionContext:aws:s3:arn": "arn:aws:s3:::bucket_name/*"
        }
    }
}
```

**Note**  
 If you're using a KMS with a CMK to encrypt your S3 bucket with S3 bucket keys enabled, set the `EncryptionContext` to the bucket ARN, not the object ARN, as in this example:  

```
"StringLike": {
    "kms:EncryptionContext:aws:s3:arn": "arn:aws:s3:::bucket_name"
}
```

The following policy statement allows every Amazon File Cache in your account to link to a specific S3 bucket\.

```
{
    "Sid": "Allow access through S3 for the FSx SLR to use the KMS key on the objects in the given S3 bucket",
    "Effect": "Allow",
    "Principal": {
        "AWS": "*"
    },
    "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
    ],
    "Resource": "*",
    "Condition": {
        "StringEquals": {
            "kms:CallerAccount": "aws_account_id",
            "kms:ViaService": "s3.bucket-region.amazonaws.com"
        },
        "StringLike": {
            "aws:userid": "*:FSx",
            "kms:EncryptionContext:aws:s3:arn": "arn:aws:s3:::bucket_name/*"
        }
    }
}
```

## Accessing server\-side encrypted Amazon S3 buckets in a different AWS account<a name="s3-server-side-cross-account-support"></a>

After you create an Amazon File Cache file system linked to an encrypted Amazon S3 bucket, you must then grant the `AWSServiceRoleForFSxS3Access_fc-01234567890` service\-linked role \(SLR\) access to the KMS key used to encrypt the S3 bucket before reading or writing data from the linked S3 bucket\. You can use an IAM role which already has permissions to the KMS key\.

**Note**  
This IAM role must be in the account that the Amazon File Cache was created in \(which is the same account as the S3 SLR\), not the account that the KMS key/S3 bucket belong to\.

You use the IAM role to call the following AWS KMS API to create a grant for the S3 SLR so that the SLR gains permission to the S3 objects\. In order to find the ARN associated with your SLR, search your IAM roles using your cache ID as the search string\.

```
$ aws kms create-grant --region cache_account_region \
      --key-id arn:aws:kms:s3_bucket_account_region:s3_bucket_account:key/key_id \
      --grantee-principal arn:aws:iam::cache_account_id:role/aws-service-role/s3.data-source.lustre.fsx.amazonaws.com/AWSServiceRoleForFSxS3Access_file-cache-id \
      --operations "Decrypt" "Encrypt" "GenerateDataKey" "GenerateDataKeyWithoutPlaintext" "CreateGrant" "DescribeKey" "ReEncryptFrom" "ReEncryptTo"
```

For more information about service\-linked roles, see [Using service\-linked roles for Amazon File Cache](using-service-linked-roles.md)\.