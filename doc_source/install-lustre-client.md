# Installing the Lustre client<a name="install-lustre-client"></a>

To mount your cache from a Linux instance, first install the open\-source Lustre client\. Amazon File Cache version 2\.12 supports access from the 2\.12 versions of the Lustre client\. Then, depending on your operating system version, use one of the following procedures\.

If your compute instance isn't running the Linux kernel specified in the installation instructions, and you can't change the kernel, you can build your own Lustre client\. For more information, see [Compiling Lustre](http://wiki.lustre.org/Compiling_Lustre) on the Lustre Wiki\.

## CentOS, Rocky Linux, and Red Hat<a name="lustre-client-rhel"></a>

### To install the Lustre client on CentOS, Rocky Linux, and Red Hat 8\.4 and newer<a name="install-lustre-client-RH8.4"></a>

You can install and update Lustre client packages that are compatible with Red Hat Enterprise Linux \(RHEL\), Rocky Linux, and CentOS from the Lustre client yum package repository\. These packages are signed to help ensure that they have not been tampered with before or during download\. The repository installation fails if you don't install the corresponding public key on your system\.

**To add the Lustre client yum package repository**

1. Open a terminal on your client\.

1. Install the AWS Lustre rpm public key by using the following command\.

   ```
   curl https://fsx-lustre-client-repo-public-keys.s3.amazonaws.com/fsx-rpm-public-key.asc -o /tmp/fsx-rpm-public-key.asc
   ```

1. Import the key by using the following command\.

   ```
   sudo rpm --import /tmp/fsx-rpm-public-key.asc
   ```

1. Add the repository and update the package manager using the following command\.

   ```
   sudo curl https://fsx-lustre-client-repo.s3.amazonaws.com/el/8/fsx-lustre-client.repo -o /etc/yum.repos.d/aws-fsx.repo
   ```

**To configure the Lustre client yum repository**

The Lustre client yum package repository is configured by default to install the Lustre client that is compatible with the kernel version that initially shipped with the latest supported CentOS, Rocky Linux, and RHEL 8 release\. To install a Lustre client that is compatible with the kernel version you are using, you can edit the repository configuration file\.

This section describes how to determine which kernel you are running, whether you need to edit the repository configuration, and how to edit the configuration file\.

1. Determine which kernel is currently running on your compute instance by using the following command\.

   ```
   uname -r
   ```

1. Do one of the following:
   + If the command returns `4.18.0-372*`, you don't need to modify the repository configuration\. Continue to the **To install the Lustre client** procedure\.
   +  If the command returns `4.18.0-348*`, you must edit the repository configuration so that it points to the Lustre client for the CentOS, Rocky Linux, and RHEL 8\.5 release\.
   +  If the command returns `4.18.0-305*`, you must edit the repository configuration so that it points to the Lustre client for the CentOS, Rocky Linux, and RHEL 8\.4 release\.

1. Edit the repository configuration file to point to a specific version of RHEL using the following command\.

   ```
   sudo sed -i 's#8#specific_RHEL_version#' /etc/yum.repos.d/aws-fsx.repo
   ```

   For example, to point to release 8\.5, substitute `specific_RHEL_version` with `8.5` in the command\.

   ```
   sudo sed -i 's#8#8.5#' /etc/yum.repos.d/aws-fsx.repo
   ```

1. Use the following command to clear the yum cache\.

   ```
   sudo yum clean all
   ```

**To install the Lustre client**
+ Install the packages from the repository using the following command\.

  ```
  sudo yum install -y kmod-lustre-client lustre-client
  ```

#### Additional information \(CentOS, Rocky Linux, and Red Hat 8\.4 and newer\)<a name="lustre-client-RH8.4-additional-info"></a>

The commands preceding install the two packages that are necessary for mounting and interacting with your cache\. The repository includes additional Lustre packages, such as a package containing the source code and packages containing tests, and you can optionally install them\. To list all available packages in the repository, use the following command\. 

```
yum --disablerepo="*" --enablerepo="aws-fsx" list available
```

To download the source rpm, containing a tarball of the upstream source code and the set of patches that we've applied, use the following command\.

```
 sudo yumdownloader --source kmod-lustre-client
```

When you run yum update, a more recent version of the module is installed if available and the existing version is replaced\. To prevent the currently installed version from being removed on update, add a line like the following to your `/etc/yum.conf` file\.

```
installonlypkgs=kernel, kernel-PAE, installonlypkg(kernel), installonlypkg(kernel-module), 
              installonlypkg(vm), multiversion(kernel), kmod-lustre-client
```

 This list includes the default install only packages, specified in the `yum.conf` man page, and the `kmod-lustre-client` package\.

### To install the Lustre client on CentOS and Red Hat 7\.9 \(x86\_64 instances\)<a name="install-lustre-client-Centos-7"></a>

You can install and update Lustre client packages that are compatible with Red Hat Enterprise Linux \(RHEL\) and CentOS from the Lustre client yum package repository\. These packages are signed to help ensure they have not been tampered with before or during download\. The repository installation fails if you don't install the corresponding public key on your system\.

**To add the AWS Lustre client yum package repository**

1. Open a terminal on your client\.

1. Install the AWS Lustre rpm public key using the following command\.

   ```
   curl https://fsx-lustre-client-repo-public-keys.s3.amazonaws.com/fsx-rpm-public-key.asc -o /tmp/fsx-rpm-public-key.asc
   ```

1. Import the key using the following command\.

   ```
   sudo rpm --import /tmp/fsx-rpm-public-key.asc
   ```

1. Add the repository and update the package manager using the following command\.

   ```
   sudo curl https://fsx-lustre-client-repo.s3.amazonaws.com/el/7/fsx-lustre-client.repo -o /etc/yum.repos.d/aws-fsx.repo
   ```

The AWS Lustre client yum package repository is configured by default to install the Lustre client that is compatible with the kernel version that initially shipped with the latest supported CentOS 7 release\. If you use the `uname -r` command to determine which kernel you are running, the command should return `3.10.0-1160*`\.

**To install the Lustre client**
+ Install the Lustre client packages from the repository using the following command\.

  ```
  sudo yum install -y kmod-lustre-client lustre-client
  ```

#### Additional information \(CentOS and Red Hat 7\.9\)<a name="lustre-client-Centos-7-additional-info"></a>

The commands preceding install the two packages that are necessary for mounting and interacting with your cache\. The repository includes additional Lustre packages, such as a package containing the source code and packages containing tests, and you can optionally install them\. To list all available packages in the repository, use the following command\. 

```
yum --disablerepo="*" --enablerepo="aws-fsx" list available
```

To download the source rpm containing a tarball of the upstream source code and the set of patches that we've applied, use the following command\.

```
 sudo yumdownloader --source kmod-lustre-client
```

When you run yum update, a more recent version of the module is installed if available, and the existing version is replaced\. To prevent the currently installed version from being removed on update, add a line like the following to your `/etc/yum.conf` file\.

```
installonlypkgs=kernel, kernel-big‐mem, kernel-enterprise, kernel-smp,
              kernel-debug, kernel-unsupported, kernel-source, kernel-devel, kernel-PAE,
              kernel-PAE-debug, kmod-lustre-client
```

 This list includes the default install only packages, specified in the `yum.conf` man page, and the `kmod-lustre-client` package\.

### To install the Lustre client on CentOS 7\.9 \(Arm\-based AWS Graviton\-powered instances\)<a name="install-lustre-client-Centos-7-arm"></a>

You can install Lustre client packages from the AWS Lustre client yum package repository that are compatible with CentOS 7 for Arm\-based AWS Graviton\-powered EC2 instances\. These packages are signed to help ensure they have not been tampered with before or during download\. The repository installation fails if you don't install the corresponding public key on your system\.

**To add the AWS Lustre client yum package repository**

1. Open a terminal on your client\.

1. Install the AWS Lustre rpm public key using the following command\.

   ```
   curl https://fsx-lustre-client-repo-public-keys.s3.amazonaws.com/fsx-rpm-public-key.asc -o /tmp/fsx-rpm-public-key.asc
   ```

1. Import the key using the following command\.

   ```
   sudo rpm --import /tmp/fsx-rpm-public-key.asc
   ```

1. Add the repository and update the package manager using the following command\.

   ```
   sudo curl https://fsx-lustre-client-repo.s3.amazonaws.com/centos/7/fsx-lustre-client.repo -o /etc/yum.repos.d/aws-fsx.repo
   ```

The AWS Lustre client yum package repository is configured by default to install the Lustre client that is compatible with the kernel version that initially shipped with the latest supported CentOS 7 release\. If you use the `uname -r` command to determine which kernel you are running, the command should return `4.18.0-193*`\.

**To install the Lustre client**
+ Install the packages from the repository using the following command\.

  ```
  sudo yum install -y kmod-lustre-client lustre-client
  ```

#### Additional information \(CentOS 7\.9 for Arm\-based AWS Graviton\-powered EC2 instances\)<a name="lustre-client-Centos-7-arm-additional-info"></a>

The commands preceding install the two packages that are necessary for mounting and interacting with your cache\. The repository includes additional Lustre packages, such as a package containing the source code and packages containing tests, and you can optionally install them\. To list all available packages in the repository, use the following command\. 

```
yum --disablerepo="*" --enablerepo="aws-fsx" list available
```

To download the source rpm, containing a tarball of the upstream source code and the set of patches that we've applied, use the following command\.

```
 sudo yumdownloader --source kmod-lustre-client
```

When you run yum update, a more recent version of the module is installed if available, and the existing version is replaced\. To prevent the currently installed version from being removed on update, add a line like the following to your `/etc/yum.conf` file\.

```
installonlypkgs=kernel, kernel-big‐mem, kernel-enterprise, kernel-smp,
              kernel-debug, kernel-unsupported, kernel-source, kernel-devel, kernel-PAE,
              kernel-PAE-debug, kmod-lustre-client
```

 This list includes the default install only packages, specified in the `yum.conf` man page, and the `kmod-lustre-client` package\.

## Ubuntu<a name="lustre-client-ubuntu"></a>

### To install the Lustre client on Ubuntu 22\.04<a name="install-lustre-client-Ubuntu-22"></a>

Starting with kernel `5.15.0.1020-aws`, Ubuntu 22\.04\.1 LTS is supported for Lustre 2\.12 clients, for both x86 and Arm based instances\.

You can get Lustre packages from the Ubuntu 22\.04 AWS Lustre repository\. To validate that the contents of the repository have not been tampered with before or during download, a GNU Privacy Guard \(GPG\) signature is applied to the metadata of the repository\. Installing the repository fails unless you have the correct public GPG key installed on your system\.

1. Open a terminal on your client\.

1. Follow these steps to add the AWS Lustre client repository:

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

      The two AWS Lustre packages that are necessary for mounting and interacting with your cache are installed\. You can optionally install additional related packages such as a package containing the source code and packages containing tests that are included in the repository\.

   1. List all available packages in the repository by using the following command\. 

      ```
      sudo apt-cache search ^lustre
      ```

   1. \(Optional\) If you want your system upgrade to also always upgrade Lustre client modules, make sure that the `lustre-client-modules-aws` package is installed using the following command\.

      ```
      sudo apt install -y lustre-client-modules-aws
      ```

### To install the Lustre client on Ubuntu 20\.04<a name="install-lustre-client-Ubuntu-20"></a>

Starting with kernel `5.15.0.1020-aws`, Ubuntu 20\.04\.5 LTS is supported for Lustre 2\.12 clients, for both x86 and Arm based instances\.

You can get Lustre packages from the Ubuntu 20\.04 AWS Lustre repository\. To validate that the contents of the repository have not been tampered with before or during download, a GNU Privacy Guard \(GPG\) signature is applied to the metadata of the repository\. Installing the repository fails unless you have the correct public GPG key installed on your system\.

1. Open a terminal on your client\.

1. Follow these steps to add the AWS Lustre client Ubuntu repository:

   1. If you have not previously registered an AWS Lustre Ubuntu repository on your client instance, download and install the required public key\. Use the following command\.

      ```
      wget -O - https://fsx-lustre-client-repo-public-keys.s3.amazonaws.com/fsx-ubuntu-public-key.asc | gpg --dearmor | sudo tee /usr/share/keyrings/fsx-ubuntu-public-key.gpg >/dev/null
      ```

   1. Add the AWS Lustre package repository to your local package manager using the following command\.

      ```
      sudo bash -c 'echo "deb [signed-by=/usr/share/keyrings/fsx-ubuntu-public-key.gpg] https://fsx-lustre-client-repo.s3.amazonaws.com/ubuntu focal main" > /etc/apt/sources.list.d/fsxlustreclientrepo.list && apt-get update'
      ```

1. Determine which kernel is currently running on your client instance, and update as needed\. The Lustre client on Ubuntu 20\.04 requires kernel `5.15.0.1020-aws` or later for both x86\-based EC2 instances and Arm\-based EC2 instances powered by AWS Graviton processors\. 

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

      The two AWS Lustre packages that are necessary for mounting and interacting with your cache are installed\. You can optionally install additional related packages such as a package containing the source code and packages containing tests that are included in the repository\.

   1. List all available packages in the repository by using the following command\. 

      ```
      sudo apt-cache search ^lustre
      ```

   1. \(Optional\) If you want your system upgrade to also always upgrade Lustre client modules, make sure that the `lustre-client-modules-aws` package is installed using the following command\.

      ```
      sudo apt install -y lustre-client-modules-aws
      ```

### To install the Lustre client on Ubuntu 18\.04<a name="install-lustre-client-Ubuntu-18"></a>

Starting with kernel `5.4.0.1085-aws`, Ubuntu 18\.04\.6 LTS is supported for Lustre 2\.12 clients, for both x86 and Arm based instances\.

You can get Lustre packages from the Ubuntu 18\.04 AWS Lustre repository\. To validate that the contents of the repository have not been tampered with before or during download, a GNU Privacy Guard \(GPG\) signature is applied to the metadata of the repository\. Installing the repository fails unless you have the correct public GPG key installed on your system\.

1. Open a terminal on your client\.

1. Follow these steps to add the AWS Lustre Ubuntu repository:

   1. If you have not previously registered an AWS Lustre Ubuntu repository on your client instance, download and install the required public key\. Use the following command\.

      ```
      wget -O - https://fsx-lustre-client-repo-public-keys.s3.amazonaws.com/fsx-ubuntu-public-key.asc | gpg --dearmor | sudo tee /usr/share/keyrings/fsx-ubuntu-public-key.gpg >/dev/null
      ```

   1. Add the AWS Lustre package repository to your local package manager using the following command\.

      ```
      sudo bash -c 'echo "deb [signed-by=/usr/share/keyrings/fsx-ubuntu-public-key.gpg] https://fsx-lustre-client-repo.s3.amazonaws.com/ubuntu bionic main" > /etc/apt/sources.list.d/fsxlustreclientrepo.list && apt-get update'
      ```

1. Determine which kernel is currently running on your client instance, and update as needed\. The Lustre client on Ubuntu 18\.04 requires kernel `15.4.0.1085-aws` or later for both x86\-based EC2 instances and Arm\-based EC2 instances powered by AWS Graviton processors\. 

   1. Run the following command to determine which kernel is running\.

      ```
      uname -r
      ```

   1. Run the following command to update to the latest Ubuntu kernel and Lustre version and then reboot\.

      ```
      sudo apt install -y linux-aws lustre-client-modules-aws && sudo reboot
      ```

       If your kernel version is greater than `5.4.0.1085-aws` for both x86\-based EC2 instances and Graviton\-based EC2 instances, and you don’t want to update to the latest kernel version, you can install Lustre for the current kernel with the following command\. 

      ```
      sudo apt install -y lustre-client-modules-$(uname -r)
      ```

      The two Lustre packages that are necessary for mounting and interacting with your cache are installed\. You can optionally install additional related packages, such as a package containing the source code and packages containing tests that are included in the repository\.

   1. List all available packages in the repository by using the following command\.

      ```
      sudo apt-cache search ^lustre
      ```

   1. \(Optional\) If you want your system upgrade to also always upgrade Lustre client modules, make sure that the `lustre-client-modules-aws` package is installed using the following command\.

      ```
      sudo apt install -y lustre-client-modules-aws
      ```