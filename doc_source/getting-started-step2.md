# Step 2: Install and configure the Lustre client on your instance before mounting your cache<a name="getting-started-step2"></a>

To mount your cache from your Amazon EC2 instance, first install the Lustre 2\.12 client\.

You can get Lustre packages from the Ubuntu 22\.04 AWS Lustre client repository\. To validate that the contents of the repository have not been tampered with before or during download, a GNU Privacy Guard \(GPG\) signature is applied to the metadata of the repository\. Installing the repository fails unless you have the correct public GPG key installed on your system\.

**To download the Lustre client onto your Amazon EC2 instance**

1. Open a terminal on your client\.

1. Follow these steps to add the AWS Lustre client Ubuntu repository:

   1. If you have not previously registered an AWS Lustre client Ubuntu repository on your client instance, download and install the required public key\. Use the following command\.

      ```
      wget -O - https://fsx-lustre-client-repo-public-keys.s3.amazonaws.com/fsx-ubuntu-public-key.asc | gpg --dearmor | sudo tee /usr/share/keyrings/fsx-ubuntu-public-key.gpg >/dev/null
      ```

   1. Add the AWS Lustre package repository to your local package manager using the following command\.

      ```
      sudo bash -c 'echo "deb [signed-by=/usr/share/keyrings/fsx-ubuntu-public-key.gpg] https://fsx-lustre-client-repo.s3.amazonaws.com/ubuntu jammy main" > /etc/apt/sources.list.d/fsxlustreclientrepo.list && apt-get update'
      ```

1. Determine which kernel is currently running on your client instance, and update as needed\. The Lustre client on Ubuntu 22\.04 requires kernel `5.15.0.1020-aws` or later for both x86\-based EC2 instances and Arm\-based EC2 instances powered by AWS Graviton processors\.

   1. Run the following command to determine which kernel is running\.

      ```
      uname -r
      ```

   1. Run the following command to update to the latest Ubuntu kernel and Lustre version and then reboot\.

      ```
      sudo apt install -y linux-aws lustre-client-modules-aws && sudo reboot
      ```

       If your kernel version is greater than `5.15.0.1020-aws` for both x86\-based EC2 instances and Graviton\-based EC2 instances, and you don’t want to update to the latest kernel version, you can install Lustre for the current kernel with the following command\.

      ```
      sudo apt install -y lustre-client-modules-$(uname -r)
      ```

      The two Lustre packages that are necessary for mounting and interacting with your cache are installed\. You can optionally install additional related packages such as a package containing the source code and packages containing tests that are included in the repository\.

   1. List all available packages in the repository by using the following command\. 

      ```
      sudo apt-cache search ^lustre
      ```

   1. \(Optional\) If you want your system upgrade to also always upgrade Lustre client modules, make sure that the `lustre-client-modules-aws` package is installed using the following command\.

      ```
      sudo apt install -y lustre-client-modules-aws
      ```

For information about installing the Lustre client on other Linux distributions, see [Installing the Lustre client](install-lustre-client.md)\.

**To mount your cache**

1. Make a directory for the mount point with the following command\.

   ```
   sudo mkdir -p /mnt
   ```

1. Mount the Amazon File Cache to the directory that you created\. Use the following command and replace the following items:
   + Replace `cache_dns_name` with the actual file cache's Domain Name System \(DNS\) name\.
   + Replace `mountname` with the cache's mount name, which you can get by running the describe\-file\-caches AWS CLI command or the [DescribeFileCaches](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DescribeFileCaches.html) API operation\.

   ```
   sudo mount -t lustre -o relatime,flock cache_dns_name@tcp:/mountname /mnt
   ```

    This command mounts your cache with these options:
   +  `relatime` – Maintains `atime` \(inode access times\) data, but not for each time that a file is accessed\. With this option enabled, `atime` data is written to disk only if the file has been modified since the `atime` data was last updated \(mtime\), or if the file was last accessed more than a certain amount of time ago \(one day by default\)\. `relatime` is required for [automatic cache eviction](cache-eviction.md#auto-cache-eviction) to work properly\.
   +  `flock` – Enables file locking for your cache\. If you don't want file locking enabled, use the `mount` command without `flock`\.

1. Verify that the mount command was successful by listing the contents of the directory to which you mounted the cache `/mnt`, by using the following command\.

   ```
   ls /mnt
   import-path  lustre
   $
   ```

   You can also use the `df` command, following\.

   ```
   df
   Filesystem                      1K-blocks    Used  Available Use% Mounted on
   devtmpf                          1001808       0    1001808   0% /dev
   tmpfs                            1019760       0    1019760   0% /dev/shm
   tmpfs                            1019760     392    1019368   1% /run
   tmpfs                            1019760       0    1019760   0% /sys/fs/cgroup
   /dev/xvda1                       8376300 1263180    7113120  16% /
   123.456.789.0@tcp:/mountname  3547698816   13824 3547678848   1% /mnt
   tmpfs                             203956       0     203956   0% /run/user/1000
   ```

   The results show the Amazon File Cache resource mounted on /mnt\.