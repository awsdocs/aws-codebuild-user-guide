# AWS Elastic Beanstalk sample for CodeBuild<a name="sample-elastic-beanstalk"></a>

This sample instructs AWS CodeBuild to use Maven to produce as build output a single WAR file named `my-web-app.war`\. This sample then deploys the WAR file to the instances in an AWS Elastic Beanstalk environment\.

**Important**  
Running this sample might result in charges to your AWS account\. These include possible charges for CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, CloudWatch Logs, and Amazon EC2\. For more information, see [CodeBuild pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service pricing](http://aws.amazon.com/kms/pricing), [Amazon CloudWatch pricing](http://aws.amazon.com/cloudwatch/pricing), and [Amazon EC2 pricing](http://aws.amazon.com/ec2/pricing)\.

## Create the source code<a name="sample-elastic-beanstalk-prepare-source"></a>

In this section, you use Maven to produce the source code\. Later, you use CodeBuild to build a WAR file based on this source code\.

1. Download and install Maven\. For information, see [Downloading Apache Maven](https://maven.apache.org/download.cgi) and [Installing Apache Maven](https://maven.apache.org/install.html) on the Apache Maven website\.

1. Switch to an empty directory on your local computer or instance, and then run this Maven command\.

   ```
   mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-web-app -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
   ```

   If successful, this directory structure and files are created\.

   ```
   (root directory name)
        `-- my-web-app
              |-- pom.xml
              `-- src    
                    `-- main
                          |-- resources
                          `-- webapp
                                |-- WEB-INF
                                |     `-- web.xml
                                `-- index.jsp
   ```

1. Create a subdirectory named `.ebextensions` in the `(root directory name)/my-web-app` directory\. In the `.ebextensions` subdirectory, create a file named `fix-path.config` with this content\. 

   ```
   container_commands:
     fix_path:
       command: "unzip my-web-app.war 2>&1 > /var/log/my_last_deploy.log"
   ```

After you run Maven, continue with one of the following scenarios:
+ [Scenario A: Run CodeBuild manually and deploy to Elastic Beanstalk manually](#sample-elastic-beanstalk-manual) 
+ [Scenario B: Use CodePipeline to run CodeBuild and deploy to Elastic Beanstalk](#sample-elastic-beanstalk-codepipeline)
+ [Scenario C: Use the Elastic Beanstalk CLI to run AWS CodeBuild and deploy to an Elastic Beanstalk environment](#sample-elastic-beanstalk-eb-cli)

## Scenario A: Run CodeBuild manually and deploy to Elastic Beanstalk manually<a name="sample-elastic-beanstalk-manual"></a>

In this scenario, you create and upload the source code\. You then use the AWS CodeBuild and AWS Elastic Beanstalk consoles to build the source code, create an Elastic Beanstalk application and environment, and deploy the build output to the environment\.

### Step a1: Add files to the source code<a name="sample-elastic-beanstalk-manual-prepare"></a>

In this step, you add an Elastic Beanstalk configuration file and a buildspec file to the code in [Create the source code](#sample-elastic-beanstalk-prepare-source)\. You then upload the source code to an S3 input bucket or a CodeCommit, GitHub, or Bitbucket repository\. 

1. Create a file named `buildspec.yml` with the following contents\. Store the file in the `(root directory name)/my-web-app` directory\.

   ```
   version: 0.2
   
   phases:
     install:
       runtime-versions:
         java: corretto11
     post_build:
       commands:
         - mvn package
         - mv target/my-web-app.war my-web-app.war
   artifacts:
     files:
       - my-web-app.war
       - .ebextensions/**/*
   ```

1. Your file structure should now look like this\.

   ```
   (root directory name)
        `-- my-web-app
              |-- .ebextensions
              |     `-- fix-path.config 
              |-- src    
              |     `-- main
              |           |-- resources
              |           `-- webapp
              |                 |-- WEB-INF
              |                 |     `-- web.xml
              |                 `-- index.jsp
              |-- buildpsec.yml
              `-- pom.xml
   ```

1.  Upload the contents of the `my-web-app` directory to an S3 input bucket or a CodeCommit, GitHub, or Bitbucket repository\.
**Important**  
Do not upload `(root directory name)` or `(root directory name)/my-web-app`, just the directories and files in `(root directory name)/my-web-app`\.   
 If you are using an S3 input bucket, it must be versioned\. Be sure to create a ZIP file that contains the directory structure and files, and then upload it to the input bucket\. Do not add `(root directory name)` or `(root directory name)/my-web-app` to the ZIP file, just the directories and files in `(root directory name)/my-web-app`\. For more information, see [How to Configure Versioning on a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html#how-to-enable-disable-versioning-intro) in the *Amazon S3 Developer Guide*\. 

### Step a2: Create the build project and run the build<a name="sample-elastic-beanstalk-manual-build"></a>

In this step, you use the AWS CodeBuild console to create a build project and then run a build\. 

1. Create or choose an S3 output bucket to store the build output\. If you're storing the source code in an S3 input bucket, the output bucket must be in the same AWS region as the input bucket\.

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

   Use the AWS region selector to choose an AWS Region where CodeBuild is supported\. This must be the same Region where your S3 output bucket is stored\.

1. Create a build project and then run a build\. For more information, see [Create a build project \(console\)](create-project-console.md) and [Run a build \(console\)](run-build-console.md)\. Leave all settings at their default values, except for these settings\.
   + For **Environment**:
     + For **Environment image**, choose **Managed image**\.
     + For **Operating system**, choose **Amazon Linux 2**\.
     + For **Runtime\(s\)**, choose **Standard**\.
     + For **Image**, choose **aws/codebuild/amazonlinux2\-x86\_64\-standard:2\.0**\.
   + For **Artifacts**:
     + For **Type**, choose **Amazon S3**\.
     + For **Bucket name**, enter the name of an S3 bucket\.
     + For **Name**, enter a build output file name that's easy for you to remember\. Include the `.zip` extension\.
     + For **Artifacts packaging**, choose **Zip**\.

### Step a3: Create the application and environment and deploy<a name="sample-elastic-beanstalk-manual-deploy"></a>

In this step, you use the AWS Elastic Beanstalk console to create an application and environment\. As part of creating the environment, you deploy the build output from the previous step to the environment\.

1. Open the AWS Elastic Beanstalk console at [https://console\.aws\.amazon\.com/elasticbeanstalk](https://console.aws.amazon.com/elasticbeanstalk)\.

   Use the AWS Region selector to choose the AWS Region where your S3 output bucket is stored\.

1. Create an Elastic Beanstalk application\. For more information, see [Managing and configuring AWS Elastic Beanstalk applications](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/applications.html) in the *AWS Elastic Beanstalk Developer Guide*\.

1. Create an Elastic Beanstalk environment for this application\. For more information, see [The create new environment wizard](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-create-wizard.html) in the *AWS Elastic Beanstalk Developer Guide*\. Leave all settings at their default values, except for these settings\.
   + For **Platform**, choose **Tomcat**\.
   + For **Application code**, choose **Upload your code**, and then choose **Upload**\. For **Source code origin**, choose **Public S3 URL**, and then enter the full URL to the build output ZIP file in the output bucket\. Choose **Upload**\.

1. After Elastic Beanstalk deploys the build output to the environment, you can see the results in a web browser\. Go to the environment URL for the instance \(for example, `http://my-environment-name.random-string.region-ID.elasticbeanstalk.com`\)\. The web browser should display the text `Hello World!`\.

## Scenario B: Use CodePipeline to run CodeBuild and deploy to Elastic Beanstalk<a name="sample-elastic-beanstalk-codepipeline"></a>

In this scenario, you complete the steps to prepare and upload the source code\. You create a build project with CodeBuild and an Elastic Beanstalk application and environment with the AWS Elastic Beanstalk console\. You then use the AWS CodePipeline console to create a pipeline\. After you create the pipeline, CodePipeline builds the source code and deploys the build output to the environment\.

### Step b1: Add a buildspec file to the source code<a name="sample-elastic-beanstalk-codepipeline-prepare"></a>

In this step, you create and add a buildspec file to the code you created in [Create the source code](#sample-elastic-beanstalk-prepare-source)\. You then upload the source code to an S3 input bucket or a CodeCommit, GitHub, or Bitbucket repository\.

1. Create a file named `buildspec.yml` with the following contents\. Store the file in the `(root directory name)/my-web-app` directory\.

   ```
   version: 0.2
   
   phases:
     install:
       runtime-versions:
         java: corretto11
     post_build:
       commands:
         - mvn package
         - mv target/my-web-app.war my-web-app.war
   artifacts:
     files:
       - my-web-app.war
       - .ebextensions/**/*
     base-directory: 'target/my-web-app'
   ```

1. Your file structure should now look like this\.

   ```
   (root directory name)
        `-- my-web-app
              |-- .ebextensions
              |     `-- fix-path.config
              |-- src    
              |     `-- main
              |           |-- resources
              |           `-- webapp
              |                 |-- WEB-INF
              |                 |     `-- web.xml
              |                 `-- index.jsp
              |-- buildpsec.yml
              `-- pom.xml
   ```

1. Upload the contents of the `my-web-app` directory to an S3 input bucket or a CodeCommit, GitHub, or Bitbucket repository\.
**Important**  
Do not upload `(root directory name)` or `(root directory name)/my-web-app`, just the directories and files in `(root directory name)/my-web-app`\.   
 If you are using an S3 input bucket, it must be versioned\. Be sure to create a ZIP file that contains the directory structure and files, and then upload it to the input bucket\. Do not add `(root directory name)` or `(root directory name)/my-web-app` to the ZIP file, just the directories and files in `(root directory name)/my-web-app`\. For more information, see [How to Configure Versioning on a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html#how-to-enable-disable-versioning-intro) in the *Amazon S3 Developer Guide*\. 

### Step b2: Create a build project<a name="sample-elastic-beanstalk-codepipeline-buildproject"></a>

In this step, you create an AWS CodeBuild build project to use with your pipeline\.

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. Create a build project\. For more information, see [Create a build project \(console\)](create-project-console.md) and [Run a build \(console\)](run-build-console.md)\. Leave all settings at their default values, except for these settings\.
   + For **Environment**:
     + For **Environment image**, choose **Managed image**\.
     + For **Operating system**, choose **Amazon Linux 2**\.
     + For **Runtime\(s\)**, choose **Standard**\.
     + For **Image**, choose **aws/codebuild/amazonlinux2\-x86\_64\-standard:2\.0**\.
   + For **Artifacts**:
     + For **Type**, choose **Amazon S3**\.
     + For **Bucket name**, enter the name of an S3 bucket\.
     + For **Name**, enter a build output file name that's easy for you to remember\. Include the `.zip` extension\.
     + For **Artifacts packaging**, choose **Zip**\.

### Step b3: Create an Elastic Beanstalk application and environment<a name="sample-elastic-beanstalk-codepipeline-beanstalk"></a>

In this step, you create an Elastic Beanstalk application and environment to use with CodePipeline\.

1. Open the Elastic Beanstalk console at [https://console\.aws\.amazon\.com/elasticbeanstalk/](https://console.aws.amazon.com/elasticbeanstalk/)\.

1. Use the AWS Elastic Beanstalk console to create an application\. For more information, see [Managing and configuring AWS Elastic Beanstalk applications](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/applications.html) in the *AWS Elastic Beanstalk Developer Guide*\.

1.  Use the AWS Elastic Beanstalk console to create an environment\. For more information, see [The create new environment wizard](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-create-wizard.html) in the *AWS Elastic Beanstalk Developer Guide*\. Except for **Platform**, leave all settings at their default values\. For **Platform**, choose **Tomcat**\.

### Step b4: Create the pipeline and deploy<a name="sample-elastic-beanstalk-codepipeline-deploy"></a>

In this step, you use the AWS CodePipeline console to create a pipeline\. After you create and run the pipeline, CodePipeline uses CodeBuild to build the source code\. CodePipeline then uses Elastic Beanstalk to deploy the build output to the environment\.

1. Create or identify a service role that CodePipeline, CodeBuild, and Elastic Beanstalk can use to access resources on your behalf\. For more information, see [Prerequisites](how-to-create-pipeline.md#how-to-create-pipeline-prerequisites)\.

1. Open the CodePipeline console at [https://console\.aws\.amazon\.com/codesuite/codepipeline/home](https://console.aws.amazon.com/codesuite/codepipeline/home)\.

   Use the AWS Region selector to choose an AWS Region where CodeBuild is supported\. If you're storing the source code in an S3 input bucket, the output bucket must be in the same AWS region as the input bucket\.

1. Create a pipeline\. For information, see [Create a pipeline that uses CodeBuild \(CodePipeline console\)](how-to-create-pipeline.md#how-to-create-pipeline-console)\. Leave all settings at their default values, except for these settings\.
   + On **Add build stage**, for **Build provider**, choose **AWS CodeBuild**\. For **Project name**, choose the build project you just created\.
   + On **Add deploy stage**, for **Deploy provider**, choose **AWS Elastic Beanstalk**\.
     + For **Application name**, choose the Elastic Beanstalk application you just created\.
     + For **Environment name**, choose the environment you just created\.

1. After the pipeline has run successfully, you can see the results in a web browser\. Go to the environment URL for the instance \(for example, `http://my-environment-name.random-string.region-ID.elasticbeanstalk.com`\)\. The web browser should display the text `Hello World!`\.

Now, whenever you make changes to the source code and upload those changes to the original S3 input bucket or to the CodeCommit, GitHub, or Bitbucket repository, CodePipeline detects the change and runs the pipeline again\. This causes CodeBuild to rebuild the code and then causes Elastic Beanstalk to deploy the rebuilt output to the environment\.

## Scenario C: Use the Elastic Beanstalk CLI to run AWS CodeBuild and deploy to an Elastic Beanstalk environment<a name="sample-elastic-beanstalk-eb-cli"></a>

In this scenario, you complete the steps to prepare and upload the source code\. You then run the Elastic Beanstalk CLI to create an Elastic Beanstalk application and environment, use CodeBuild to build the source code, and then deploy the build output to the environment\. For more information, see [Using the EB CLI with CodeBuild](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli-codebuild.html) in the *AWS Elastic Beanstalk Developer Guide*\.

### Step c1: Add files to the source code<a name="sample-elastic-beanstalk-eb-cli-prepare"></a>

In this step, you add an Elastic Beanstalk configuration file and a buildspec file to the code you created in [Create the source code](#sample-elastic-beanstalk-prepare-source)\. You also create or identify a service role for the buildspec file\.

1. Create or identify a service role that Elastic Beanstalk and the CLI can use on your behalf\. For information, see [Create a CodeBuild service role](setting-up.md#setting-up-service-role)\.

1. Create a file named `buildspec.yml` with the following contents\. Store the file in the `(root directory name)/my-web-app` directory\.

   ```
   version: 0.2
   
   phases:
     install:
       runtime-versions:
         java: corretto11
     post_build:
       commands:
         - mvn package
         - mv target/my-web-app.war my-web-app.war
   artifacts:
     files:
       - my-web-app.war
       - .ebextensions/**/*
   eb_codebuild_settings:
     CodeBuildServiceRole: my-service-role-name
     ComputeType: BUILD_GENERAL1_SMALL
     Image: aws/codebuild/standard:4.0
     Timeout: 60
   ```

   In the preceding code, replace *my\-service\-role\-name* with the name of the service role you created or identified earlier\. 

1. Your file structure should now look like this\.

   ```
   (root directory name)
        `-- my-web-app
              |-- .ebextensions
              |     `-- fix-path.config 
              |-- src    
              |     `-- main
              |           |-- resources
              |           `-- webapp
              |                 |-- WEB-INF
              |                 |     `-- web.xml
              |                 `-- index.jsp
              |-- buildpsec.yml
              `-- pom.xml
   ```

### Step c2: Install and run the EB CLI<a name="sample-elastic-beanstalk-eb-cli-run"></a>

1. If you have not already done so, install and configure the EB CLI on the same computer or instance where you created the source code\. For information, see [Install the Elastic Beanstalk command line interface \(EB CLI\)](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html) and [Configure the EB CLI](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-configuration.html) in the *AWS Elastic Beanstalk Developer Guide*\.

1. From the command line or terminal, run the cd command or similar to switch to your `(root directory name)/my-web-app` directory\. Run the eb init command to configure the EB CLI\.

   ```
   eb init
   ```

   When prompted:
   + Choose an AWS Region where AWS CodeBuild is supported and where you want to create your Elastic Beanstalk application and environment\.
   + Create an Elastic Beanstalk application, and enter a name for the application\.
   + Choose the `Tomcat` platform\.
   + Choose the `Tomcat 8 Java 8` version\.
   + Choose whether you want to use SSH to set up access to your environment's instances\.

1.  From the same directory, run the eb create command to create an Elastic Beanstalk environment\.

   ```
   eb create
   ```

   When prompted:
   + Enter the name for the environment or accept the suggested name\.
   + Enter the DNS CNAME prefix for the environment or accept the suggested value\.
   + For this sample, accept the Classic load balancer type\.

1. After you run the eb create command, the EB CLI does the following:

   1. Creates a ZIP file from the source code and then uploads the ZIP file to an S3 bucket in your account\.

   1. Creates an Elastic Beanstalk application and application version\.

   1. Creates a CodeBuild project\.

   1. Runs a build based on the new project\.

   1. Deletes the project after the build is complete\.

   1. Creates an Elastic Beanstalk environment\.

   1. Deploys the build output to the environment\.

1. After the EB CLI deploys the build output to the environment, you can see the results in a web browser\. Go to the environment URL for the instance \(for example, `http://my-environment-name.random-string.region-ID.elasticbeanstalk.com`\)\. The web browser should display the text `Hello World!`\.

If you want, you can make changes to the source code and then run the eb deploy command from the same directory\. The EB CLI performs the same steps as the eb create command, but it deploys the build output to the existing environment instead of creating a new environment\.

## Related resources<a name="acb-more-info"></a>
+ For information about getting started with AWS CodeBuild, see [Getting started with AWS CodeBuild using the console](getting-started.md)\.
+ For information about troubleshooting issues in CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.
+ For information about quotas in CodeBuild, see [Quotas for AWS CodeBuild](limits.md)\.