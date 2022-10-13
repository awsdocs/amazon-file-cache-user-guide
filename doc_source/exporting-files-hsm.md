# Exporting files using HSM commands<a name="exporting-files-hsm"></a>

To export an individual file to your data repository and verify that the file has successfully been exported to your data repository, you can run the commands shown following\. A return value of `states: (0x00000009) exists archived` indicates that the file has successfully been exported\.

```
sudo lfs hsm_archive path/to/export/file
sudo lfs hsm_state path/to/export/file
```

To export changes on an entire cache or an entire directory in your cache, run the following commands\. If you export multiple files simultaneously, Amazon File Cache exports your files to your data repository in parallel\.

```
nohup find local/directory -type f -print0 | xargs -0 -n 1 sudo lfs hsm_archive &
```

To determine whether the export has completed, run the following command\.

```
find path/to/export/file -type f -print0 | xargs -0 -n 1 -P 8 sudo lfs hsm_action | grep "ARCHIVE" | wc -l
```

If the command returns with zero files remaining, the export is complete\.