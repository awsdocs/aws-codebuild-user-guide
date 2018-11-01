--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# AWS Elastic Beanstalk Sample for AWS CodeBuild<a name="sample-elastic-beanstalk"></a>

This sample instructs AWS CodeBuild to use Maven to produce as build output a single WAR file named `my-web-app.war`\. This sample then deploys the WAR file to the instances in an Elastic Beanstalk environment\.

**Important**  
Running this sample might result in charges to your AWS account\. These include possible charges for AWS CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, CloudWatch Logs, and Amazon EC2\. For more information, see [AWS CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing), and [Amazon EC2 Pricing](http://aws.amazon.com/ec2/pricing)\.

## Create the Source Code<a name="sample-elastic-beanstalk-prepare-source"></a>

In this section, you use Maven to produce the source code\. Later on, you use AWS CodeBuild to build a WAR file based on this source code\.

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

After you run Maven, continue with one of the following scenarios:
+ [Scenario A: Run AWS CodeBuild Manually and Deploy to Elastic Beanstalk Manually](#sample-elastic-beanstalk-manual) 
+ [Scenario B: Use AWS CodePipeline to Run AWS CodeBuild and Deploy to Elastic Beanstalk](#sample-elastic-beanstalk-codepipeline)
+ [Scenario C: Use the Elastic Beanstalk CLI to Run AWS CodeBuild and Deploy to an Elastic Beanstalk Environment](#sample-elastic-beanstalk-eb-cli)

## Scenario A: Run AWS CodeBuild Manually and Deploy to Elastic Beanstalk Manually<a name="sample-elastic-beanstalk-manual"></a>

In this scenario, you create and upload the source code\. You then use the AWS CodeBuild and Elastic Beanstalk consoles to build the source code, create an Elastic Beanstalk application and environment, and deploy the build output to the environment\.

### Step A1: Add Files to the Source Code<a name="sample-elastic-beanstalk-manual-prepare"></a>

In this step, you add an Elastic Beanstalk configuration file and a build spec file to the code in [Create the Source Code](#sample-elastic-beanstalk-prepare-source)\. You then upload the source code to an Amazon S3 input bucket or an AWS CodeCommit or GitHub repository \.

1. Create a subdirectory named `.ebextensions` in the `(root directory name)/my-web-app` directory\. In the `.ebextensions` subdirectory, create a file named `fix-path.config` with this content\. 

   ```
   container_commands:
     fix_path:
       command: "unzip my-web-app.war 2>&1 > /var/log/my_last_deploy.log"
   ```

1. Create a file named `buildspec.yml` with the following contents\. Store the file in the `(root directory name)/my-web-app` directory\.

   ```
   version: 0.2
   
   phases:
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

1. Upload the contents of the `my-web-app` directory to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\.
**Important**  
Do not upload `(root directory name)` or `(root directory name)/my-web-app`, just the directories and files in `(root directory name)/my-web-app`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the directory structure and files, and then upload it to the input bucket\. Do not add `(root directory name)` or `(root directory name)/my-web-app` to the ZIP file, just the directories and files in `(root directory name)/my-web-app`\.

### Step A2: Create the Build Project and Run the Build<a name="sample-elastic-beanstalk-manual-build"></a>

In this step, you use the AWS CodeBuild console to create a build project and then run a build\. 

1. Create or identify an Amazon S3 output bucket to store the build output\. If you're storing the source code in an Amazon S3 input bucket, the output bucket must be in the same AWS region as the input bucket\.

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/](https://console.aws.amazon.com/codesuite/codebuild/)\.

   Use the AWS region selector to choose a region that supports AWS CodeBuild and matches the region where your Amazon S3 output bucket is stored\.

1. Create a build project and then run a build\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console) and [Run a Build \(Console\)](run-build.md#run-build-console)\. Leave all settings at their default values, except for these settings\.
   + For **Environment**:
     + For **Environment image**, choose **Managed image**\.
     + For **Operating system**, choose **Ubuntu**\.
     + For **Runtime**, choose **Java**\.
     + For **Runtime version**, choose **aws/codebuild/java:openjdk\-8**\.
   + For **Artifacts**:
     + For **Name**, enter a build output file name that's easy for you to remember\. Include the `.zip` extension\.
     + For **Artifacts packaging**, choose **Zip**\.

### Step A3: Create the Application and Environment and Deploy<a name="sample-elastic-beanstalk-manual-deploy"></a>

In this step, you use the Elastic Beanstalk console to create an application and environment\. As part of creating the environment, you deploy the build output from the previous step to the environment\.

1. Open the Elastic Beanstalk console at [https://console\.aws\.amazon\.com/elasticbeanstalk](https://console.aws.amazon.com/elasticbeanstalk)\.

   Use the AWS region selector to choose the region where your Amazon S3 output bucket is stored\.

1. Create an Elastic Beanstalk application\. For more information, see [Managing and Configuring AWS Elastic Beanstalk Applications](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/applications.html)\.

1. Create an Elastic Beanstalk environment for this application\. For more information, see [The Create New Environment Wizard](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-create-wizard.html)\. Leave all settings at their default values, except for these settings\.
   + For **Platform**, choose **Tomcat**\.
   + For **Application code**, choose **Upload your code**, and then choose **Upload**\. For **Source code origin**, choose **Public S3 URL**, and then enter the full URL to the build output ZIP file in the output bucket\. Choose **Upload**\.

1. After Elastic Beanstalk deploys the build output to the environment, you can see the results in a web browser\. Go to the environment URL for the instance \(for example, `http://my-environment-name.random-string.region-ID.elasticbeanstalk.com`\)\. The web browser should display the text `Hello World!`\.

## Scenario B: Use AWS CodePipeline to Run AWS CodeBuild and Deploy to Elastic Beanstalk<a name="sample-elastic-beanstalk-codepipeline"></a>

In this scenario, you complete the steps to prepare and upload the source code\. You create a build project with AWS CodeBuild and an AWS Elastic Beanstalk application and environment with the AWS Elastic Beanstalk console\. You then use the AWS CodePipeline console to create a pipeline\. After you create the pipeline, AWS CodePipeline builds the source code and deploys the build output to the environment\.

### Step B1: Add a Build Spec File to the Source Code<a name="sample-elastic-beanstalk-codepipeline-prepare"></a>

In this step, you create and add a build spec file to the code you created in [Create the Source Code](#sample-elastic-beanstalk-prepare-source)\. You then upload the source code to an Amazon S3 input bucket or an AWS CodeCommit or GitHub repository\.

1. Create a file named `buildspec.yml` with the following contents\. Store the file in the `(root directory name)/my-web-app` directory\.

   ```
   version: 0.2
   
   phases:
     post_build:
       commands:
         - mvn package
   artifacts:
     files:
       - '**/*'
     base-directory: 'target/my-web-app'
   ```

1. Your file structure should now look like this\.

   ```
   (root directory name)
        `-- my-web-app
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

1. Upload this contents of the `my-web-app` directory to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\.
**Important**  
Do not upload `(root directory name)` or `(root directory name)/my-web-app`, just the directories and files in `(root directory name)/my-web-app`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the directory structure and files, and then upload it to the input bucket\. Do not add `(root directory name)` or `(root directory name)/my-web-app` to the ZIP file, just the directories and files in `(root directory name)/my-web-app`\.

### Step B2: Create a build project<a name="sample-elastic-beanstalk-codepipeline-buildproject"></a>

In this step, you create an AWS CodeBuild build project to use with your pipeline\.

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/](https://console.aws.amazon.com/codesuite/codebuild/)\.

1. Create a build project\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console) and [Run a Build \(Console\)](run-build.md#run-build-console)\. Leave all settings at their default values, except for these settings\.
   + For **Environment**:
     + For **Environment image**, choose **Managed image**\.
     + For **Operating system**, choose **Ubuntu**\.
     + For **Runtime**, choose **Java**\.
     + For **Runtime version**, choose **aws/codebuild/java:openjdk\-8**\.
   + For **Artifacts**:
     + For **Name**, enter a build output file name that's easy for you to remember\. Include the `.zip` extension\.
     + For **Artifacts packaging**, choose **Zip**\.

### Step B3: Create an AWS Elastic Beanstalk application and environment<a name="sample-elastic-beanstalk-codepipeline-beanstalk"></a>

In this step, you create an AWS Elastic Beanstalk application and environment to use with AWS CodePipeline\.

1. Open the Elastic Beanstalk console at [https://console\.aws\.amazon\.com/elasticbeanstalk/](https://console.aws.amazon.com/elasticbeanstalk/)\.

1. Use the Elastic Beanstalk console to create an application\. For more information, see [Managing and Configuring AWS Elastic Beanstalk Applications](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/applications.html)\.

1.  Use the Elastic Beanstalk console to create an environment\. For more information, see [The Create New Environment Wizard](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-create-wizard.html)\. Except for **Platform**, leave all settings at their default values\. For **Platform**, choose **Tomcat**\.

### Step B4: Create the Pipeline and Deploy<a name="sample-elastic-beanstalk-codepipeline-deploy"></a>

In this step, you use the AWS CodePipeline console to create a pipeline\. After you create and run the pipeline, AWS CodePipeline uses AWS CodeBuild to build the source code\. AWS CodePipeline then uses Elastic Beanstalk to deploy the build output to the environment\.

1. Create or identify a service role that AWS CodePipeline, AWS CodeBuild, and Elastic Beanstalk can use to do their work on your behalf\. For more information, see [Prerequisites](how-to-create-pipeline.md#how-to-create-pipeline-prerequisites)\.

1. Open the AWS CodePipeline console at [https://console\.aws\.amazon\.com/codesuite/codepipeline](https://console.aws.amazon.com/codesuite/codepipeline)\.

   Use the AWS region selector to choose a region that supports AWS CodeBuild and, if you're storing the source code in an Amazon S3 input bucket, choose the region where your input bucket is stored\.

1. Create a pipeline\. For information, see [Create a Pipeline That Uses AWS CodeBuild \(AWS CodePipeline Console\)](how-to-create-pipeline.md#how-to-create-pipeline-console)\. Leave all settings at their default values, except for these settings\.
   + For **Step 3: Add build stage**, for **Build provider**, choose AWS CodeBuild\. For **Project name**, choose the build project you just created\.
   + For **Step 4: Add deploy stage**, for **Deployment provider**, choose **AWS Elastic Beanstalk**\.
     + For **Application name**, choose the Elastic Beanstalk application you just created\.
     + For **Environment name**, choose the environment you just created\.

1. After the pipeline has run successfully, you can see the results in a web browser\. Go to the environment URL for the instance \(for example, `http://my-environment-name.random-string.region-ID.elasticbeanstalk.com`\)\. The web browser should display the text `Hello World!`\.

Now, whenever you make changes to the source code and upload those changes to the original Amazon S3 input bucket or to the AWS CodeCommit, GitHub, or Bitbucket repository, AWS CodePipeline detects the change and runs the pipeline again\. This causes AWS CodeBuild to rebuild the code and then causes Elastic Beanstalk to deploy the rebuilt output to the environment\.

## Scenario C: Use the Elastic Beanstalk CLI to Run AWS CodeBuild and Deploy to an Elastic Beanstalk Environment<a name="sample-elastic-beanstalk-eb-cli"></a>

In this scenario, you complete the steps to prepare and upload the source code\. You then run the Elastic Beanstalk CLI to create an Elastic Beanstalk application and environment, use AWS CodeBuild to build the source code, and then deploy the build output to the environment\. For more information, see [Using the EB CLI with AWS CodeBuild](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli-codebuild.html) in the *AWS Elastic Beanstalk Developer Guide*\.

### Step C1: Add Files to the Source Code<a name="sample-elastic-beanstalk-eb-cli-prepare"></a>

In this step, you add an Elastic Beanstalk configuration file and a build spec file to the code you created in [Create the Source Code](#sample-elastic-beanstalk-prepare-source)\. You also create or identify a service role for the build spec file\.

1. Create or identify a service role that Elastic Beanstalk and the CLI can use on your behalf\. For information, see [Create an AWS CodeBuild Service Role](setting-up.md#setting-up-service-role)\.

1. Create a subdirectory named `.ebextensions` in the `(root directory name)/my-web-app` directory\. In the `.ebextensions` subdirectory, create a file named `fix-path.config` with this content\. 

   ```
   container_commands:
     fix_path:
       command: "unzip my-web-app.war 2>&1 > /var/log/my_last_deploy.log"
   ```

1. Create a file named `buildspec.yml` with the following contents\. Store the file in the `(root directory name)/my-web-app` directory\.

   ```
   version: 0.2
   
   phases:
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
     Image: aws/codebuild/java:openjdk-8
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

### Step C2: Install and Run the EB CLI<a name="sample-elastic-beanstalk-eb-cli-run"></a>

1. If you have not already done so, install and configure the EB CLI on the same computer or instance where you created the source code\. For information, see [Install the Elastic Beanstalk Command Line Interface \(EB CLI\)](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html) and [Configure the EB CLI](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-configuration.html) in the *AWS Elastic Beanstalk Developer Guide*\.

1. From your computer's or instance's command line or terminal, run the cd command or similar to switch to your `(root directory name)/my-web-app` directory\. Run the eb init command to configure the EB CLI\.

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

   1. Creates a ZIP file from the source code and then uploads the ZIP file to an Amazon S3 bucket in your account\.

   1. Creates an Elastic Beanstalk application and application version\.

   1. Creates an AWS CodeBuild project\.

   1. Runs a build based on the new project\.

   1. Deletes the project after the build is complete\.

   1. Creates an Elastic Beanstalk environment\.

   1. Deploys the build output to the environment\.

1. After the EB CLI deploys the build output to the environment, you can see the results in a web browser\. Go to the environment URL for the instance \(for example, `http://my-environment-name.random-string.region-ID.elasticbeanstalk.com`\)\. The web browser should display the text `Hello World!`\.

If you want, you can make changes to the source code and then run the eb deploy command from the same directory\. The EB CLI performs the same steps as the eb create command, but it deploys the build output to the existing environment instead of creating a new environment\.

## Related Resources<a name="w4aac11c45c43c15"></a>
+ For more information about getting started with AWS CodeBuild, see [Getting Started with AWS CodeBuild](getting-started.md)\.
+ For more information about troubleshooting problems with AWS CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.
+ For more information about limits in AWS CodeBuild, see [Limits for AWS CodeBuild](limits.md)\.