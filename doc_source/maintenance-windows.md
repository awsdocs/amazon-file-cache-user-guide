# Amazon File Cache maintenance windows<a name="maintenance-windows"></a>

Amazon File Cache performs routine software patching for the Lustre software it manages\. The maintenance window is your opportunity to control what day and time of the week this software patching occurs\.

Patching occurs infrequently, typically once every several weeks\. Patching should require only a fraction of your 30\-minute maintenance window\. During these few minutes of time, your cache will be temporarily unavailable\. 

You choose the maintenance window during cache creation\. If you have no time preference, then a 30\-minute default window is assigned\.

You can use the AWS Management Console, AWS CLI or one of the AWS SDKs to change the maintenance window for your caches\.

**To change the maintenance window using the console**

1. Open the AWS Management Console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. Choose **Caches** in the navigation pane\.

1. Choose the cache that you want to change the maintenance window for\. The **Summary** details page appears\.

1. Choose the **Maintenance** tab\. The maintenance window **Settings** panel appears\.

1. Choose **Update** and enter the new day and time that you want the maintenance window to start\.

1. Choose **Save** to save your changes\. The new maintenance start time is displayed in the **Settings** panel\.

You can use the AWS CLI or one of the AWS SDKs to change the maintenance window for your cache using the [UpdateFileCache](https://docs.aws.amazon.com/fsx/latest/APIReference/API_UpdateFileCache.html) operation\.

Run the following command, replacing the `--file-cache-id` value with the ID for your cache, and the date and time with when you want to begin the window\.

```
aws fsx update-file-cache --file-cache-id fc-01234567890123456 --lustre-configuration WeeklyMaintenanceStartTime=1:01:30
```