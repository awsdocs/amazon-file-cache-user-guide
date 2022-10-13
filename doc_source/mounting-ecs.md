# Mounting from Amazon Elastic Container Service<a name="mounting-ecs"></a>

You can access your cache from an Amazon Elastic Container Service \(Amazon ECS\) Docker container on an Amazon EC2 instance\. You can do so by using either of the following options:

1. By mounting your cache from the Amazon EC2 instance that is hosting your Amazon ECS tasks, and exporting this mount point to your containers\.

1. By mounting the cache directly inside your task container\.

For more information about Amazon ECS, see [What is Amazon Elastic Container Service?](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html) in the *Amazon Elastic Container Service Developer Guide*\.

We recommend using option 1 \([Mounting from an Amazon EC2 instance hosting Amazon ECS tasks](#mounting-from-ecs-ec2)\) because it provides better resource use, especially if you start many containers \(more than five\) on the same EC2 instance or if your tasks are short\-lived \(less than 5 minutes\)\. 

Use option 2 \([Mounting from a Docker container](#mounting-from-docker)\), if you're unable to configure the EC2 instance, or if your application requires the container's flexibility\.

**Note**  
Mounting your cache on an AWS Fargate launch type isn't supported\.

The following sections describe the procedures for each of the options for mounting your cache from an Amazon ECS container\.

**Topics**
+ [Mounting from an Amazon EC2 instance hosting Amazon ECS tasks](#mounting-from-ecs-ec2)
+ [Mounting from a Docker container](#mounting-from-docker)

## Mounting from an Amazon EC2 instance hosting Amazon ECS tasks<a name="mounting-from-ecs-ec2"></a>

This procedure shows how you can configure an Amazon ECS on EC2 instance to locally mount your cache\. The procedure uses `volumes` and `mountPoints` container properties to share the resource and make this cache accessible to locally running tasks\. For more information, see [Launching an Amazon ECS container instance](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/launch_container_instance.html) in the *Amazon Elastic Container Service Developer Guide*\. 

This procedure is for an Amazon ECS\-Optimized Amazon Linux 2 AMI\. If you are using another Linux distribution, see [Installing the Lustre client](install-lustre-client.md)\.

**To mount your cache from Amazon ECS on an EC2 instance**

1. When launching Amazon ECS instances, either manually or using an Auto Scaling group, add the lines in the following code example to the end of the **User data** field\. Replace the following items in the example:
   + Replace `cache_dns_name` with the actual cache's DNS name\.
   + Replace `mountname` with the cache's mount name\.
   + Replace `mountpoint` with the cache's mount point, which you need to create\.

   ```
   #!/bin/bash
   
   ...<existing user data>...
   
   fsx_dnsname=cache_dns_name
   fsx_mountname=mountname
   fsx_mountpoint=mountpoint
   amazon-linux-extras install -y lustre2.10
   mkdir -p "$fsx_mountpoint"
   mount -t lustre ${fsx_dnsname}@tcp:/${fsx_mountname} ${fsx_mountpoint} -o relatime,flock
   ```

1. When creating your Amazon ECS tasks, add the following `volumes` and `mountPoints` container properties in the JSON definition\. Replace `mountpoint` with the cache's mount point \(such as `/mnt`\)\.

   ```
   {
       "volumes": [
              {
                    "host": {
                         "sourcePath": "mountpoint"
                    },
                    "name": "Lustre"
              }
       ],
       "mountPoints": [
              {
                    "containerPath": "mountpoint",
                    "sourceVolume": "Lustre"
              }
       ],
   }
   ```

## Mounting from a Docker container<a name="mounting-from-docker"></a>

The following procedure shows how you can configure an Amazon ECS task container to install the `lustre-client` package and mount your cache in it\. The procedure uses an Amazon Linux \(`amazonlinux`\) Docker image, but a similar approach can work for other distributions\.

**To mount your cache from a Docker container**

1. Install the `lustre-client` package and mount your cache with the `command` property\. Replace the following items in the example:
   + Replace `cache_dns_name` with the actual file cache's DNS name\.
   + Replace `mountname` with the cache's mount name\.
   + Replace `mountpoint` with the cache's mount point\.

   ```
   "command": [
     "/bin/sh -c \"amazon-linux-extras install -y lustre2.10; mount -t lustre cache_dns_name@tcp:/mountname mountpoint -o relatime,flock;\""
   ],
   ```

1. Add `SYS_ADMIN` capability to your container to authorize it to mount your cache, using the `linuxParameters` property\.

   ```
   "linuxParameters": {
     "capabilities": {
         "add": [
           "SYS_ADMIN"
         ]
      }
   }
   ```