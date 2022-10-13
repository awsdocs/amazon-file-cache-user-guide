# Step 3: Run your analysis<a name="getting-started-step3"></a>

Now that your cache has been created and mounted to a compute instance, you can use it to run your high\-performance compute workload\. The workload will load data from the Amazon S3 data repository as files are accessed by your workload\.

After you've run the workload, you can export data that you've written to your cache back to your Amazon S3 bucket at any time\. From a terminal on one of your compute instances, run the following command to export a file to your Amazon S3 bucket\.

```
sudo lfs hsm_archive file_name
```

For more information on how to run this command on a folder or large collection of files quickly, see [Exporting files using HSM commands](exporting-files-hsm.md)\.