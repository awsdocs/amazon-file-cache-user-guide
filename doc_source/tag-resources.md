# Tag your Amazon File Cache resources<a name="tag-resources"></a>

To help you manage your cache resources such as caches and data repository associations \(DRAs\), you can assign your own metadata to each resource in the form of tags\. Tags enable you to categorize your AWS resources in different ways, for example, by purpose, owner, or environment\. This is useful when you have many resources of the same type—you can quickly identify a specific resource based on the tags that you've assigned to it\. This topic describes tags and shows you how to create them\.

**Topics**
+ [Tag basics](#tag-basics)
+ [Tagging your resources](#tagging-your-resources)
+ [Tag restrictions](#tag-restrictions)
+ [Permissions and tag](#tags-iam)

## Tag basics<a name="tag-basics"></a>

A tag is a label that you assign to an AWS resource\. Each tag consists of a key and an optional value, both of which you define\.

Tags enable you to categorize your AWS resources in different ways, for example, by purpose, owner, or environment\. For example, you could define a set of tags for your account's Amazon File Cache resources that helps you track each cache's owner and stack level\.

We recommend that you devise a set of tag keys that meets your needs for each resource type\. Using a consistent set of tag keys makes it easier for you to manage your resources\. You can search and filter the resources based on the tags you add\.

Tags don't have any semantic meaning to Amazon File Cache and are interpreted strictly as a string of characters\. Also, tags are not automatically assigned to your resources\. You can edit tag keys and values, and you can remove tags from a resource at any time\. You can set the value of a tag to an empty string, but you can't set the value of a tag to null\. If you add a tag that has the same key as an existing tag on that resource, the new value overwrites the old value\. If you delete a resource, any tags for the resource are also deleted\.

If you're using the Amazon File Cache API, the AWS CLI, or an AWS SDK, you can use the `TagResource` API action to apply tags to existing resources\. Additionally, some resource\-creating actions enable you to specify tags for a resource when the resource is created\. If tags cannot be applied during resource creation, we roll back the resource creation process\. This ensures that resources are either created with tags or not created at all, and that no resources are left untagged at any time\. By tagging resources at the time of creation, you can eliminate the need to run custom tagging scripts after resource creation\. For more information about enabling users to tag resources on creation, see [Grant permission to tag resources during creation](using-tags.md#supported-iam-actions-tagging)\.

## Tagging your resources<a name="tagging-your-resources"></a>

You can tag Amazon File Cache resources that exist in your account\. If you're using the AWS Management Console, you can apply tags to resources by using the Tags tab on the relevant resource screen\. When you create resources, you can apply the Name key with a value, and you can apply tags of your choice when creating a new cache\. The console may organize resources according to the Name tag, but this tag doesn't have any semantic meaning to the Amazon File Cache service\.

You can apply tag\-based resource\-level permissions in your IAM policies to the Amazon File Cache API actions that support tagging on creation to implement granular control over the users and groups that can tag resources on creation\. Your resources are properly secured from creation—tags are applied immediately to your resources, therefore any tag\-based resource\-level permissions controlling the use of resources are immediately effective\. Your resources can be tracked and reported on more accurately\. You can enforce the use of tagging on new resources, and control which tag keys and values are set on your resources\.

You can also apply resource\-level permissions to the `TagResource` and `UntagResource` Amazon File Cache API actions in your IAM policies to control which tag keys and values are set on your existing resources\.

For more information about tagging your resources for billing, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) in the *AWS Billing User Guide*\.

## Tag restrictions<a name="tag-restrictions"></a>

The following basic restrictions apply to tags:
+ Maximum number of tags per resource – 50
+ For each resource, each tag key must be unique, and each tag key can have only one value\.
+ Maximum key length – 128 Unicode characters in UTF\-8
+ Maximum value length – 256 Unicode characters in UTF\-8
+ The allowed characters for Amazon File Cache tags are: letters, numbers, and spaces representable in UTF\-8, and the following characters: \+ \- = \. \_ : / @\.
+ Tag keys and values are case\-sensitive\.
+ The `aws:` prefix is reserved for AWS use\. If a tag has a tag key with this prefix, then you can't edit or delete the tag's key or value\. Tags with the `aws:` prefix do not count against your tags per resource limit\.

You can't delete a resource based solely on its tags; you must specify the resource identifier\. For example, to delete a cache that you tagged with a tag key called `DeleteMe`, you must use the `DeleteFileCache` action with the cache resource identifier, such as fc\-1234567890abcdef0\.

When you tag public or shared resources, the tags you assign are available only to your AWS account; no other AWS account will have access to those tags\. For tag\-based access control to shared resources, each AWS account must assign its own set of tags to control access to the resource\.

## Permissions and tag<a name="tags-iam"></a>

For more information about the permissions required to tag Amazon File Cache resources at creation, see [Grant permission to tag resources during creation](using-tags.md#supported-iam-actions-tagging)\. For more information about using tags to restrict access to Amazon File Cache resources in IAM policies, see [Using tags to control access to your Amazon File Cache resources](using-tags.md#restrict-fsx-access-tags)\.