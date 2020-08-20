# Amazon Elastic File System sample for AWS CodeBuild<a name="sample-efs"></a>

 You might want to create your AWS CodeBuild builds on Amazon Elastic File System, a scalable, shared file service for Amazon EC2 instances\. The storage capacity with Amazon EFS is elastic, so it grows or shrinks as files are added and removed\. It has a simple web services interface that you can use to create and configure file systems\. It also manages all of the file storage infrastructure for you, so you do not need to worry about deploying, patching, or maintaining file system configurations\. For more information, see [What is Amazon Elastic File System?](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html) in the *Amazon Elastic File System User Guide*\. 

 This sample shows you how to configure a CodeBuild project so that it mounts and then builds a Java application to an Amazon EFS file system\. Before you begin, you must have a Java application ready to build that is uploaded to an S3 input bucket or an AWS CodeCommit, GitHub, GitHub Enterprise Server, or Bitbucket repository\. 

Data in transit for your file system is encrypted\. To encrypt data in transit using a different image, see [Encrypting data in transit](https://docs.aws.amazon.com/efs/latest/ug/encryption-in-transit.html)\. 

## High\-level steps<a name="sample-efs-high-level-steps"></a>

 This sample covers the three high\-level steps required to use Amazon EFS with AWS CodeBuild: 

1.  Create a virtual private cloud \(VPC\) in your AWS account\. 

1.  Create a file system that uses this VPC\. 

1.  Create and build a CodeBuild project that uses the VPC\. The CodeBuild project uses the following to identify the file system:
   +  A unique file system identifier\. You choose the identifier when you specify the file system in your build project\.
   + The file system ID\. The ID is displayed when you view your file system in the Amazon EFS console\.
   +  A mount point\. This is a directory in your Docker container that mounts the file system\. 
   + Mount options\. These include details about how to mount the file system\.

**Note**  
 A file system created in Amazon EFS is supported on Linux platforms only\. 

## Create a VPC using AWS CloudFormation<a name="sample-efs-create-vpc"></a>

 Create your VPC with an AWS CloudFormation template\. 

1.  Follow the instructions in [AWS CloudFormation VPC template](cloudformation-vpc-template.md) to use AWS CloudFormation to create a VPC\. 
**Note**  
 The VPC created by this AWS CloudFormation template has two private subnets and two public subnets\. You must only use private subnets when you use AWS CodeBuild to mount the file system you created in Amazon EFS\. If you use one of the public subnets, the build fails\. 

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1.  Choose the VPC you created with AWS CloudFormation\.

1. On the **Description** tab, make a note of the name of your VPC and its ID\. Both are required when you create your AWS CodeBuild project later in this sample\. 

## Create an Amazon Elastic File System file system with your VPC<a name="sample-efs-create-efs"></a>

 Create a simple Amazon EFS file system for this sample using the VPC you created earlier\. 

1. Sign in to the AWS Management Console and open the Amazon EFS console at [ https://console\.aws\.amazon\.com/efs/](https://console.aws.amazon.com/efs/)\.

1.  Choose **Create file system**\. 

1.  From **VPC**, choose the VPC name you noted earlier in this sample\. 

1.  Leave the Availability Zones associated with your subnets selected\. 

1.  Choose **Next Step**\. 

1.  In **Add tags**, for the default **Name** key, in **Value**, enter the name of your Amazon EFS file system\. 

1.  Keep **Bursting** and **General Purpose** selected as your default performance and throughput modes, and then choose **Next Step**\. 

1. For **Configure client access**, choose **Next Step**\.

1.  Choose **Create File System**\. 

## Create a CodeBuild project to use with Amazon EFS<a name="sample-efs-create-acb"></a>

 Create a AWS CodeBuild project that uses the VPC you created earlier in this sample\. When the build is run, it mounts the Amazon EFS file system created earlier\. Next, it stores the \.jar file created by your Java application in your file system's mount point directory\.

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  From the navigation pane, choose **Build projects**, and then choose **Create build project**\. 

1.  In **Project name**, enter a name for your project\. 

1.  From **Source provider**, choose the repository that contains the Java application you want to build\. 

1.  Enter information, such as a repository URL, that CodeBuild uses to locate your application\. The options are different for each source provider\. For more information, see [Choose source provider](create-project-console.md#create-project-source-provider)\. 

1.  From **Environment image**, choose **Managed image**\. 

1.  From **Operating system**, choose **Amazon Linux 2**\. 

1. From **Runtime\(s\)**, choose **Standard**\. 

1.  From **Image**, choose **aws/codebuild/amazonlinux2\-x86\_64\-standard:3\.0**\. 

1.  From **Environment type**, choose **Linux**\. 

1.  Select **Privileged**\. 
**Note**  
By default, Docker containers do not allow access to any devices\. Privileged mode grants a build project's Docker container access to all devices\. For more information, see [Runtime Privilege and Linux Capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.

1.  Under **Service role**, choose **New service role**\. In **Role name**, enter a name for the role CodeBuild creates for you\. 

1. Expand **Additional configuration**\.

1.  From **VPC**, choose the VPC ID\. 

1.  From **Subnets**, choose one or more of the private subnets associated with your VPC\. You must use private subnets in a build that mounts an Amazon EFS file system\. If you use a public subnet, the build fails\. 

1.  From **Security Groups**, choose the default security group\.

1.  In **File systems**, enter the following information:
   +  For **Identifier**, enter a unique file system identifier\. It must be fewer than 129 characters and contain only alphanumeric characters and underscores\. CodeBuild uses this identifier to create an environment variable that identifies the elastic file system\. The environment variable format is `CODEBUILD_file-system-identifier` in capital letters\. For example, if you enter **efs\-1**, the environment variable is `CODEBUILD_EFS-1`\. 
   +  For **ID**, choose the file system ID\. 
   +  \(Optional\) Enter a directory in the file system\. CodeBuild mounts this directory\. If you leave **Directory path** blank, CodeBuild mounts the entire file system\. The path is relative to the root of the file system\. 
   +  For **Mount point**, enter the absolute path of the directory in your build container where the file system is mounted\. If this directory does not exist, CodeBuild creates it during the build\. 
   +  \(Optional\) Enter mount options\. If you leave **Mount options** blank, CodeBuild uses its default mount options \(`nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2`\)\. For more information, see [Recommended NFS Mount Options](https://docs.aws.amazon.com/efs/latest/ug/mounting-fs-nfs-mount-settings.html) in the *Amazon Elastic File System User Guide*\. 

1.  For **Build specification**, choose **Insert build commands**, and then choose **Switch to editor**\. 

1.  Enter the following buildspec commands into the editor\. Replace `file-system-identifier` with the identifier you entered in step 17\. Use capital letters \(for example, `CODEBUILD_EFS-1`\)\.

   ```
   version: 0.2
   phases:
     install:
       runtime-versions:
         java: corretto11    
     build:
       commands:
         - mvn compile -Dgpg.skip=true -Dmaven.repo.local=$CODEBUILD_file-system-identifier
   ```

1.  Use the default values for all other settings, and then choose **Create build project**\. When your build is complete, the console page for your project is displayed\. 

1.  Choose **Start build**\. 

## CodeBuild and Amazon EFS sample summary<a name="sample-efs-summary"></a>

 After your AWS CodeBuild project is built: 
+  You have a \.jar file created by your Java application that is built to your Amazon EFS file system under your mount point directory\. 
+  An environment variable that identifies your file system is created using the file system identifier you entered when you created the project\. 

 For more information, see [Mounting file systems](https://docs.aws.amazon.com/efs/latest/ug/mounting-fs.html) in the *Amazon Elastic File System User Guide*\. 

## Troubleshooting<a name="sample-efs-troubleshooting"></a>

The following are errors you might encounter when setting up EFS with CodeBuild\.

**Topics**
+ [CLIENT\_ERROR: mounting '127\.0\.0\.1:/' failed\. permission denied](#sample-efs-troubleshooting.permission-denied)
+ [CLIENT\_ERROR: mounting '127\.0\.0\.1:/' failed\. connection reset by peer](#sample-efs-troubleshooting.connection-reset)
+ [VPC\_CLIENT\_ERROR: Unexpected EC2 error: UnauthorizedOperation](#sample-efs-troubleshooting.unauthorized-operation)

### CLIENT\_ERROR: mounting '127\.0\.0\.1:/' failed\. permission denied<a name="sample-efs-troubleshooting.permission-denied"></a>

When using a custom EFS file system policy, you must first establish a trust relationship between EFS and CodeBuild by doing one of the following:
+ Add `codebuild.amazonaws.com` as a trusted service in the Principal in the EFS file system policy, 
+ Add the `elasticfilesystem:ClientMount` action to the CodeBuild project service role policy\.

### CLIENT\_ERROR: mounting '127\.0\.0\.1:/' failed\. connection reset by peer<a name="sample-efs-troubleshooting.connection-reset"></a>

There are two possible causes for this error:
+ The CodeBuild VPC subnet is in a different availability zone than the EFS mount target\. You can resolve this by adding a VPC subnet in the same availability zone as the EFS mount target\.
+ The security group does not have permissions to communicate with EFS\. You can resolve this by adding an inbound rule to allow all traffic from either the VPC \(add the primary CIDR block for your VPC\), or the security group itself\.

### VPC\_CLIENT\_ERROR: Unexpected EC2 error: UnauthorizedOperation<a name="sample-efs-troubleshooting.unauthorized-operation"></a>

This error occurs when all of the subnets in your VPC configuration for the CodeBuild project are public subnets\. You must have at least one private subnet in the VPC to ensure network connectivity\. 