--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Amazon Elastic File System Sample for AWS CodeBuild<a name="sample-efs"></a>

 You might want to create your AWS CodeBuild builds on Amazon EFS\. Amazon EFS is a scalable, shared file service for Amazon EC2 instances\. The storage capacity with Amazon EFS is elastic, so it grows or shrinks as files are added and removed\. It has a simple web services interface that you can use to create and configure file systems\. It also manages all of the file storage infrastructure for you, so you do not need to worry about deploying, patching, or maintaining file system configurations\. For more information, see [What Is Amazon Elastic File System](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html)\. 

 This sample shows you how to configure an AWS CodeBuild project so that it mounts and then builds a Java application to an Amazon EFS file system\. Before you begin, you must have a Java application ready to build that is uploaded to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, GitHub Enterprise, or Bitbucket repository\. You can use the instructions in [Maven in 5 Minutes Sample for AWS CodeBuild ](sample-maven-5m.md) to create a Java application\. 

**Note**  
 If you use [Maven in 5 Minutes Sample for AWS CodeBuild ](sample-maven-5m.md) to create a sample Java application, you can skip the step 3 \(the step that creates the file `buildspec.yml`\)\. In this sample you do not need a buildspec file because you use the buildspec editor to enter build commands required to mount your Amazon EFS file system\. 

## Amazon Elastic File System and AWS CodeBuild Sample High\-Level Steps<a name="sample-efs-high-level-steps"></a>

 This sample covers the three high\-level steps required to use Amazon EFS with AWS CodeBuild: 

1.  Create an Amazon VPC\. 

1.  Create an Amazon EFS that uses this Amazon VPC\. 

1.  Create and build an AWS CodeBuild project that uses the Amazon VPC\. Instructions that include how to mount an Amazon Elastic File System file system are entered into the buildspec editor when you build the project\. 

## Create an Amazon VPC Using AWS CloudFormation<a name="sample-efs-create-vpc"></a>

 Create your Amazon VPC with an AWS CloudFormation template\. 

1.  Follow the instructions here, [AWS CloudFormation VPC Template](cloudformation-vpc-template.md), to use AWS CloudFormation to create an Amazon VPC\. If you are already familiar with AWS CloudFormation, you can go directly to the AWS CloudFormation console to create a stack using the template available for download from [https://s3.amazonaws.com/codebuild-cloudformation-templates-public/vpc_cloudformation_template.yml](https://s3.amazonaws.com/codebuild-cloudformation-templates-public/vpc_cloudformation_template.yml)\. For more information, see the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide//Welcome.html)\. 
**Note**  
 The Amazon VPC created by this AWS CloudFormation template has two private subnets and two public subnets\. You must only use private subnets when you use AWS CodeBuild to mount Amazon EFS\. If you use one of the public subnets, the build fails\. 

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1.  Choose the Amazon VPC you created with AWS CloudFormation\. 

1.  Make a note of the VPC ID displayed on the **Summary** tab\. This ID is required when you create your AWS CodeBuild project later in this sample\. 

## Create an Amazon Elastic File System File System with Your Amazon VPC<a name="sample-efs-create-efs"></a>

 Create a simple Amazon EFS file system for this sample using the Amazon VPC you created earlier\. 

1. Sign in to the AWS Management Console and open the Amazon EFS console at [ https://console\.aws\.amazon\.com/efs/](https://console.aws.amazon.com/efs/)\.

1.  Choose **Create file system**\. 

1.  From **VPC**, choose the VPC ID you noted earlier in this sample\. 

1.  Leave the Availability Zones associated with your subnets selected\. 

1.  Choose **Next Step**\. 

1.  In **Add tags**, for the default **Name** key, in **Value**, enter the name of your Amazon EFS file system\. 

1.  Keep **General Purpose** and **Bursting** selected as your default performance and throughput modes, and then choose **Next Step**\. 

1.  Choose **Create File System**\. 

1. Choose the name of the file system you created from the list\. Make a note of the DNS name\. You enter this in the buildspec file that is used to build your AWS CodeBuild project\. 

## Create an AWS CodeBuild Project to Use with Amazon EFS<a name="sample-efs-create-acb"></a>

 Create an AWS CodeBuild project that uses the Amazon VPC you created earlier in this sample\. This AWS CodeBuild project does not use a source and does not create an artifact\. When the build is run, it mounts the Amazon EFS file system created earlier in this sample and caches the Maven dependency to it\. 

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  From the navigation pane, choose **Build projects**, and then choose **Create build project**\. 

1.  In **Project name**, enter a name for your project\. 

1.  From **Source provider**, choose the repository that contains the Java application you want to build\. 

1.  Enter information, such as a repository URL, that AWS CodeBuild uses to locate your application\. The options are different for each source provider\. For more information, see [Choose source provider](create-project.md#create-project-source-provider)\. 

1.  From **Environment image**, choose **Managed image**\. 

1.  From **Operating system**, choose **Ubuntu**\. 

1.  From **Runtime**, choose **Java**\. 

1.  From **Runtime version** choose **aws/codebuild/java:openjdk\-8**\. 

1.  Select **Privileged**\. 

1.  Under **Service role**, choose **New service role**\. In **Role name**, enter a name for the role AWS CodeBuild creates for you\. 

1.  For **Build specification**, choose **Insert build commands** and then choose **Switch to editor**\. 

1.  Enter the following buildspec commands into the editor\. For the `EFS_DNS`, enter the DNS name of your file system\. 

   ```
   version: 0.2
   
   env:
     variables:
         EFS_DIR: "/efs"
         M3_HOME: ".m2"
         EFS_DNS: "fs-11223344.efs.us-east-1.amazonaws.com"
   phases:
     install:
       commands:
         - mkdir -p $EFS_DIR
         - apt-get update && apt-get install -y nfs-common
     pre_build:
       commands:
         - mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2 $EFS_DNS:/ $EFS_DIR
         - df -h
         - mkdir -p $EFS_DIR/$M3_HOME/
     build:
       commands:
         - mvn compile -Dgpg.skip=true -Dmaven.repo.local=$EFS_DIR/$M3_HOME/
   ```

1. Expand **Additional configuration**\.

1.  From **VPC**, choose the VPC ID\. 

1.  From **Subnets**, choose one or more of the private subnets associated with your Amazon VPC\. You must use private subnets in a build that mounts an Amazon EFS file system\. If you use a public subnet, the build fails\. 

1.  From **Security Groups**, choose the security group that works with your Amazon VPC\. 

1.  Use the default values for all other settings, and then choose **Create build project**\. When your build is complete, you are on the console page for your project\. 

1.  Choose **Start build**\. 

## AWS CodeBuild and Amazon EFS Sample Summary<a name="sample-efs-summary"></a>

 After your AWS CodeBuild project is built, you have a \.jar file created by your Java application\. The \.jar file is built to your Amazon EFS file system in a directory called `/efs/.m2`\. AWS CodeBuild uses the mount command, `mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2`, to mount the Amazon EFS file system\. For more information, see [Mounting File Systems](https://docs.aws.amazon.com/efs/latest/ug/mounting-fs.html)\. 