# Step 4: Clean up resources<a name="getting-started-step4"></a>

After you have finished this exercise, you should follow these steps to clean up your resources and protect your AWS account\.

**To clean up resources**

1. On the Amazon EC2 console, terminate your instance\. For more information, see [Terminate your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html) in the *Amazon EC2 User Guide for Linux Instances\.*

1. On the AWS Management Console, delete your cache with the following procedure:

   1. In the navigation pane, choose **Caches**\.

   1. Choose the cache that you want to delete from list of caches on the dashboard\.

   1. For **Actions**, choose **Delete cache**\.

   1. In the dialog box that appears, provide the cache ID to confirm the deletion\. Choose **Delete cache**\.

1. If you created an Amazon S3 bucket for this exercise, and if you don't want to preserve the data you exported, you can now delete it\. For more information, see [Deleting a bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html) in the *Amazon Simple Storage Service User Guide\.*