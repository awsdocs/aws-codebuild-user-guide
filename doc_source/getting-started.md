# Getting Started with CodeBuild<a name="getting-started"></a>

In this walkthrough, you use AWS CodeBuild to build a collection of sample source code input files \(called *build input artifacts* or *build input*\) into a deployable version of the source code \(called *build output artifact* or *build output*\)\. Specifically, you instruct CodeBuild to use Apache Maven, a common build tool, to build a set of Java class files into a Java Archive \(JAR\) file\. You do not need to be familiar with Apache Maven or Java to complete this walkthrough\.

**Important**  
Completing this walkthrough may result in charges to your AWS account\. These include possible charges for CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, and CloudWatch Logs\. For more information, see [CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), and [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

**Topics**
+ [Step 1: Create or Use Amazon S3 Buckets to Store the Build Input and Output](#getting-started-input-bucket)
+ [Step 2: Create the Source Code to Build](#getting-started-create-source-code)
+ [Step 3: Create the Build Spec](#getting-started-create-build-spec)
+ [Step 4: Add the Source Code and the Build Spec to the Input Bucket](#getting-started-upload-source-code)
+ [Step 5: Create the Build Project](#getting-started-create-build-project)
+ [Step 6: Run the Build](#getting-started-run-build)
+ [Step 7: View Summarized Build Information](#getting-started-monitor-build)
+ [Step 8: View Detailed Build Information](#getting-started-build-log)
+ [Step 9: Get the Build Output Artifact](#getting-started-output)
+ [Step 10: Clean Up](#getting-started-clean-up)
+ [Next Steps](#getting-started-next-steps)

## Step 1: Create or Use Amazon S3 Buckets to Store the Build Input and Output<a name="getting-started-input-bucket"></a>

To complete this walkthrough, you need two Amazon S3 buckets:
+ One of these buckets stores the build input \(the *input bucket*\)\. In this walkthrough, we name this input bucket `codebuild-region-ID-account-ID-input-bucket`, where *region\-ID* represents the AWS Region of the bucket, and *account\-ID* represents your AWS account ID\.
+ The other bucket stores the build output \(the *output bucket*\)\. In this walkthrough, we name this output bucket `codebuild-region-ID-account-ID-output-bucket`\.

If you chose a different name for either of these buckets, be sure to use it throughout this walkthrough\.

These two buckets must be in the same AWS Region as your builds\. For example, if you instruct CodeBuild to run a build in the US East \(Ohio\) Region, then these buckets must also be in the US East \(Ohio\) Region\.

To create a bucket, see [Creating a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/CreatingaBucket.html) in the *Amazon Simple Storage Service User Guide*\. 

**Note**  
You can use a single bucket for this walkthrough, but using two buckets makes it easier to see where the build input is coming from and where the build output is going\.  
Although CodeBuild also supports build input stored in CodeCommit, GitHub, and Bitbucket repositories, this walkthrough does not show you how to use them\. For more information, see [Plan a Build](planning.md)\.

## Step 2: Create the Source Code to Build<a name="getting-started-create-source-code"></a>

In this step, you create the source code that you want CodeBuild to build to the output bucket\. This source code consists of two Java class files and an Apache Maven Project Object Model \(POM\) file\.

1. In an empty directory on your local computer or instance, create this directory structure\.

   ```
   (root directory name)
       `-- src
            |-- main
            |     `-- java
            `-- test
                  `-- java
   ```

1. Using a text editor of your choice, create this file, name it `MessageUtil.java`, and then save it in the `src/main/java` directory\.

   ```
   public class MessageUtil {
     private String message;
   
     public MessageUtil(String message) {
       this.message = message;
     }
   
     public String printMessage() {
       System.out.println(message);
       return message;
     }
   
     public String salutationMessage() {
       message = "Hi!" + message;
       System.out.println(message);
       return message;
     }
   }
   ```

   This class file creates as output the string of characters passed into it\. The `MessageUtil` constructor sets the string of characters\. The `printMessage` method creates the output\. The `salutationMessage` method outputs `Hi!` followed by the string of characters\.

1. Create this file, name it `TestMessageUtil.java`, and then save it in the `/src/test/java` directory\.

   ```
   import org.junit.Test;
   import org.junit.Ignore;
   import static org.junit.Assert.assertEquals;
   
   public class TestMessageUtil {
   
     String message = "Robert";    
     MessageUtil messageUtil = new MessageUtil(message);
      
     @Test
     public void testPrintMessage() {      
       System.out.println("Inside testPrintMessage()");     
       assertEquals(message,messageUtil.printMessage());
     }
   
     @Test
     public void testSalutationMessage() {
       System.out.println("Inside testSalutationMessage()");
       message = "Hi!" + "Robert";
       assertEquals(message,messageUtil.salutationMessage());
     }
   }
   ```

   This class file sets the `message` variable in the `MessageUtil` class to `Robert`\. It then tests to see if the `message` variable was successfully set by checking whether the strings `Robert` and `Hi!Robert` appear in the output\.

1. Create this file, name it `pom.xml`, and then save it in the root \(top level\) directory\.

   ```
   <project xmlns="http://maven.apache.org/POM/4.0.0" 
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
     <modelVersion>4.0.0</modelVersion>
     <groupId>org.example</groupId>
     <artifactId>messageUtil</artifactId>
     <version>1.0</version>
     <packaging>jar</packaging>
     <name>Message Utility Java Sample App</name>
     <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.11</version>
         <scope>test</scope>
       </dependency>	
     </dependencies>
     <build>
       <plugins>
         <plugin>
           <groupId>org.apache.maven.plugins</groupId>
           <artifactId>maven-compiler-plugin</artifactId>
           <version>3.8.0</version>
         </plugin>
       </plugins>
     </build>
   </project>
   ```

   Apache Maven uses the instructions in this file to convert the `MessageUtil.java` and `TestMessageUtil.java` files into a file named `messageUtil-1.0.jar` and then run the specified tests\. 

At this point, your directory structure should look like this\.

```
(root directory name)
    |-- pom.xml
    `-- src
         |-- main
         |     `-- java
         |           `-- MessageUtil.java
         `-- test
               `-- java
                     `-- TestMessageUtil.java
```

## Step 3: Create the Build Spec<a name="getting-started-create-build-spec"></a>

In this step, you create a build specification \(build spec\) file\. A *build spec* is a collection of build commands and related settings, in YAML format, that CodeBuild uses to run a build\. Without a build spec, CodeBuild cannot successfully convert your build input into build output or locate the build output artifact in the build environment to upload to your output bucket\.

Create this file, name it `buildspec.yml`, and then save it in the root \(top level\) directory\.

```
version: 0.2

phases:
  install:
    runtime-versions:
      java: openjdk11
  pre_build:
    commands:
      - echo Nothing to do in the pre_build phase...
  build:
    commands:
      - echo Build started on `date`
      - mvn install
  post_build:
    commands:
      - echo Build completed on `date`
artifacts:
  files:
    - target/messageUtil-1.0.jar
```

**Important**  
Because a build spec declaration must be valid YAML, the spacing in a build spec declaration is important\. If the number of spaces in your build spec declaration does not match this one, the build might fail immediately\. You can use a YAML validator to test whether your build spec declaration is valid YAML\. 

**Note**  
Instead of including a build spec file in your source code, you can declare build commands separately when you create a build project\. This is helpful if you want to build your source code with different build commands without updating your source code's repository each time\. For more information, see [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\.

In this build spec declaration:
+ `version` represents the version of the build spec standard being used\. This build spec declaration uses the latest version, `0.2`\.
+ `phases` represents the build phases during which you can instruct CodeBuild to run commands\. These build phases are listed here as `install`, `pre_build`, `build`, and `post_build`\. You cannot change the spelling of these build phase names, and you cannot create more build phase names\. 

  In this example, during the `build` phase, CodeBuild runs the `mvn install` command\. This command instructs Apache Maven to compile, test, and package the compiled Java class files into a build output artifact\. For completeness, a few `echo` commands are placed in each build phase in this example\. When you view detailed build information later in this walkthrough, the output of these `echo` commands can help you better understand how CodeBuild runs commands and in which order\. \(Although all build phases are included in this example, you are not required to include a build phase if you do not plan to run any commands during that phase\.\) For each build phase, CodeBuild runs each specified command, one at a time, in the order listed, from beginning to end\. 
+ `artifacts` represents the set of build output artifacts that CodeBuild uploads to the output bucket\. `files` represents the files to include in the build output\. CodeBuild uploads the single `messageUtil-1.0.jar` file found in the `target` relative directory in the build environment\. The file name `messageUtil-1.0.jar` and the directory name `target` are based on the way Apache Maven creates and stores build output artifacts for this example only\. In your own builds, these file names and directories are different\. 

For more information, see the [Build Spec Reference](build-spec-ref.md)\.

At this point, your directory structure should look like this\.

```
(root directory name)
    |-- pom.xml
    |-- buildspec.yml
    `-- src
         |-- main
         |     `-- java
         |           `-- MessageUtil.java
         `-- test
               `-- java
                     `-- TestMessageUtil.java
```

## Step 4: Add the Source Code and the Build Spec to the Input Bucket<a name="getting-started-upload-source-code"></a>

In this step, you add the source code and build spec file to the input bucket\.

Using your operating system's zip utility, create a file named `MessageUtil.zip` that includes `MessageUtil.java`, `TestMessageUtil.java`, `pom.xml`, and `buildspec.yml`\. 

The `MessageUtil.zip` file's directory structure must look like this\.

```
MessageUtil.zip
    |-- pom.xml
    |-- buildspec.yml
    `-- src
         |-- main
         |     `-- java
         |           `-- MessageUtil.java
         `-- test
               `-- java
                     `-- TestMessageUtil.java
```

**Important**  
Do not include the `(root directory name)` directory, only the directories and files in the `(root directory name)` directory\.

Upload the `MessageUtil.zip` file to the input bucket named `codebuild-region-ID-account-ID-input-bucket`\. 

**Important**  
For CodeCommit, GitHub, and Bitbucket repositories, by convention, you must store a build spec file named `buildspec.yml` in the root \(top level\) of each repository or include the build spec declaration as part of the build project definition\. Do not create a ZIP file that contains the repository's source code and build spec file\.   
For build input stored in Amazon S3 buckets only, you must create a ZIP file that contains the source code and, by convention, a build spec file named `buildspec.yml` at the root \(top level\) or include the build spec declaration as part of the build project definition\.  
If you want to use a different name for your build spec file, or you want to reference a build spec in a location other than the root, you can specify a build spec override as part of the build project definition\. For more information, see [Build Spec File Name and Storage Location](build-spec-ref.md#build-spec-ref-name-storage)\.

## Step 5: Create the Build Project<a name="getting-started-create-build-project"></a>

In this step, you create a build project that AWS CodeBuild uses to run the build\. A *build project* defines how CodeBuild runs a build\. It includes information such as where to get the source code, the build environment to use, the build commands to run, and where to store the build output\. A *build environment* represents a combination of operating system, programming language runtime, and tools that CodeBuild uses to run a build\. The build environment is expressed as a Docker image\. \(For more information, see the [Docker Overview](https://docs.docker.com/engine/docker-overview/) topic on the Docker Docs website\.\) For this build environment, you instruct CodeBuild to use a Docker image that contains a version of the Java Development Kit \(JDK\) and Apache Maven\.

You can use the [CodeBuild console](#getting-started-create-build-project-console) or [AWS CLI](#getting-started-create-build-project-cli) to complete this step\.

**Note**  
You can work with CodeBuild in several ways: through the CodeBuild console, AWS CodePipeline, the AWS CLI, or the AWS SDKs\. This walkthrough demonstrates how to use the CodeBuild console and the AWS CLI\. To learn how to use CodePipeline, see [Use AWS CodePipeline with CodeBuild](how-to-create-pipeline.md)\. To learn how to use the AWS SDKs, see [Run AWS CodeBuild Directly](how-to-run.md)\. <a name="getting-started-create-build-project-console"></a>

**To create the build project \(console\)**

1. Sign in to the AWS Management Console and open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the AWS region selector, choose a region that supports CodeBuild\. For more information, see [CodeBuild](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the "Regions and Endpoints" topic in the *Amazon Web Services General Reference*\.

1.  If a CodeBuild information page is displayed, choose **Create project**\. Otherwise, on the navigation pane, expand **Build**, and then choose **Build projects**\. 

1. On the **Create build project** page, in **Project configuration**, for **Project name**, enter a name for this build project \(in this example, `codebuild-demo-project`\)\. Build project names must be unique across each AWS account\. If you use a different name, be sure to use it throughout this walkthrough\.
**Note**  
On the **Create build project** page, you might see an error message similar to the following: **User: *user\-ARN* is not authorized to perform: codebuild:ListProjects**\. This is most likely because you signed in to the AWS Management Console as an IAM user that does not have sufficient permissions to use CodeBuild in the console\. To fix this, sign out of the AWS Management Console, and then sign back in with credentials belonging to one of the following IAM entities:   
Your AWS root account\. This is not recommended\. For more information, see [The Account Root User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in the *IAM User Guide*\.
An administrator IAM user in your AWS account\. For more information, see [Creating Your First IAM Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
An IAM user in your AWS account with the AWS managed policies named **AWSCodeBuildAdminAccess**, **AmazonS3ReadOnlyAccess**, and **IAMFullAccess** attached to that IAM user or to an IAM group that the IAM user belongs to\. If you do not have an IAM user or group in your AWS account with these permissions, and you cannot add these permissions to your IAM user or group, contact your AWS account administrator for assistance\. For more information, see [AWS Managed \(Predefined\) Policies for CodeBuild](auth-and-access-control-iam-identity-based-access-control.md#managed-policies)\.

1. In **Source**, for **Source provider**, choose **Amazon S3**\.

1. For **Bucket**, choose **codebuild\-*region\-ID*\-*account\-ID*\-input\-bucket**\.

1.  For **S3 object key**, enter **MessageUtil\.zip**\.

1. In **Environment**, for **Environment image**, leave **Managed image** selected\.

1. For **Operating system**, choose **Ubuntu**\.

1. For **Runtime**, choose **Standard**\.

1. For **Image**, choose **aws/codebuild/standard:2\.0**\.

1. In **Service role**, leave **New service role** selected, and leave **Role name** unchanged\.

1. For **Buildspec**, leave **Use a buildspec file** selected\.

1. In **Artifacts**, for **Type**, choose **Amazon S3**\.

1. For **Bucket name**, choose **codebuild\-*region\-ID*\-*account\-ID*\-output\-bucket**\.

1. Leave **Name** and **Path** blank\.

1. Choose **Create build project**\.

   Skip ahead to [Step 6: Run the Build](#getting-started-run-build)\.<a name="getting-started-create-build-project-cli"></a>

**To create the build project \(AWS CLI\)**

1. Use the AWS CLI to run the create\-project command:

   ```
   aws codebuild create-project --generate-cli-skeleton
   ```

   JSON\-formatted data appears in the output\. Copy the data to a file named `create-project.json` in a location on the local computer or instance where the AWS CLI is installed\. If you choose to use a different file name, be sure to use it throughout this walkthrough\.

   Modify the copied data to follow this format, and then save your results:

   ```
   {
     "name": "codebuild-demo-project",
     "source": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-input-bucket/MessageUtil.zip"
     },
     "artifacts": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-output-bucket"
     },
     "environment": {
       "type": "LINUX_CONTAINER",
       "image": "aws/codebuild/standard:2.0",
       "computeType": "BUILD_GENERAL1_SMALL"
     },
     "serviceRole": "serviceIAMRole"
   }
   ```

   Replace *serviceIAMRole* with the Amazon Resource Name \(ARN\) of a CodeBuild service role \(for example, `arn:aws:iam::account-ID:role/role-name`\)\. To create one, see [Create a CodeBuild Service Role](setting-up.md#setting-up-service-role)\.

   In this data:
   + `name` represents a required identifier for this build project \(in this example, `codebuild-demo-project`\)\. Build project names must be unique across all build projects in your account\. 
   + For `source`, `type` is a required value that represents the source code's repository type \(in this example, `S3` for an Amazon S3 bucket\)\.
   + For `source`, `location` represents the path to the source code \(in this example, the input bucket name followed by the ZIP file name\)\.
   + For `artifacts`, `type` is a required value that represents the build output artifact's repository type \(in this example, `S3` for an Amazon S3 bucket\)\.
   + For `artifacts`, `location` represents the name of the output bucket you created or identified earlier \(in this example, `codebuild-region-ID-account-ID-output-bucket`\)\.
   + For `environment`, `type` is a required value that represents the type of build environment \(`LINUX_CONTAINER` is currently the only allowed value\)\.
   + For `environment`, `image` is a required value that represents the Docker image name and tag combination this build project uses, as specified by the Docker image repository type \(in this example, `aws/codebuild/standard:2.0` for a Docker image in the CodeBuild Docker images repository\)\. `aws/codebuild/standard` is the name of the Docker image\. `1.0` is the tag of the Docker image\. 

     To find more Docker images you can use in your scenarios, see the [Build Environment Reference](build-env-ref.md)\.
   + For `environment`, `computeType` is a required value that represents the computing resources CodeBuild uses \(in this example, `BUILD_GENERAL1_SMALL`\)\.
**Note**  
Other available values in the original JSON\-formatted data, such as `description`, `buildspec`, `auth` \(including `type` and `resource`\), `path`, `namespaceType`, `name` \(for `artifacts`\), `packaging`, `environmentVariables` \(including `name` and `value`\), `timeoutInMinutes`, `encryptionKey`, and `tags` \(including `key` and `value`\) are optional\. They are not used in this walkthrough, so they are not shown here\. For more information, see [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli)\.

1. Switch to the directory that contains the file you just saved, and then run the create\-project command again\.

   ```
   aws codebuild create-project --cli-input-json file://create-project.json
   ```

   If successful, data similar to this appears in the output\.

   ```
   {
     "project": {
       "name": "codebuild-demo-project",
       "serviceRole": "serviceIAMRole",
       "tags": [],
       "artifacts": {
         "packaging": "NONE",
         "type": "S3",
         "location": "codebuild-region-ID-account-ID-output-bucket",
         "name": "message-util.zip"
       },
       "lastModified": 1472661575.244,
       "timeoutInMinutes": 60,
       "created": 1472661575.244,
       "environment": {
         "computeType": "BUILD_GENERAL1_SMALL",
         "image": "aws/codebuild/standard:2.0",
         "type": "LINUX_CONTAINER",
         "environmentVariables": []
       },
       "source": {
         "type": "S3",
         "location": "codebuild-region-ID-account-ID-input-bucket/MessageUtil.zip"
       },
       "encryptionKey": "arn:aws:kms:region-ID:account-ID:alias/aws/s3",
       "arn": "arn:aws:codebuild:region-ID:account-ID:project/codebuild-demo-project"
     }
   }
   ```
   + `project` represents information about this build project\.
     + `tags` represents any tags that were declared\.
     + `packaging` represents how the build output artifact is stored in the output bucket\. `NONE` means that a folder is created in the output bucket\. The build output artifact is stored in that folder\.
     + `lastModified` represents the time, in Unix time format, when information about the build project was last changed\.
     + `timeoutInMinutes` represents the number of minutes after which CodeBuild stops the build if the build has not been completed\. \(The default is 60 minutes\.\)
     + `created` represents the time, in Unix time format, when the build project was created\.
     + `environmentVariables` represents any environment variables that were declared and are available for CodeBuild to use during the build\.
     + `encryptionKey` represents the ARN of the AWS KMS customer master key \(CMK\) that CodeBuild used to encrypt the build output artifact\.
     + `arn` represents the ARN of the build project\.

**Note**  
After you run the create\-project command, an error message similar to the following might be output: **User: *user\-ARN* is not authorized to perform: codebuild:CreateProject**\. This is most likely because you configured the AWS CLI with the credentials of an IAM user that does not have sufficient permissions to use CodeBuild to create build projects\. To fix this, configure the AWS CLI with credentials belonging to one of the following IAM entities:   
Your AWS root account\. This is not recommended\. For more information, see [The Account Root User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in the *IAM User Guide*\.
An administrator IAM user in your AWS account\. For more information, see [Creating Your First IAM Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
An IAM user in your AWS account with the AWS managed policies named **AWSCodeBuildAdminAccess**, **AmazonS3ReadOnlyAccess**, and **IAMFullAccess** attached to that IAM user or to an IAM group that the IAM user belongs to\. If you do not have an IAM user or group in your AWS account with these permissions, and you cannot add these permissions to your IAM user or group, contact your AWS account administrator for assistance\. For more information, see [AWS Managed \(Predefined\) Policies for CodeBuild](auth-and-access-control-iam-identity-based-access-control.md#managed-policies)\.

## Step 6: Run the Build<a name="getting-started-run-build"></a>

In this step, you instruct AWS CodeBuild to run the build with the settings in the build project\.

You can use the [CodeBuild console](#getting-started-run-build-console) or [AWS CLI](#getting-started-run-build-cli) to complete this step\.<a name="getting-started-run-build-console"></a>

**To run the build \(console\)**

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the navigation pane, choose **Build projects**\.

1. In the list of build projects, choose **codebuild\-demo\-project**, and then choose **Start build**\. 

1. On the **Start new build** page, choose **Start build**\.

1. Skip ahead to [Step 7: View Summarized Build Information](#getting-started-monitor-build)\.<a name="getting-started-run-build-cli"></a>

**To run the build \(AWS CLI\)**

1. Use the AWS CLI to run the start\-build command:

   ```
   aws codebuild start-build --project-name project-name
   ```

   Replace *project\-name* with your build project name from the previous step \(for example, `codebuild-demo-project`\)\.

1. If successful, data similar to the following appears in the output:

   ```
   {
     "build": { 
       "buildComplete": false,
       "initiator": "user-name",   
       "artifacts": { 
         "location": "arn:aws:s3:::codebuild-region-ID-account-ID-output-bucket/message-util.zip"
       },
       "projectName": "codebuild-demo-project",
       "timeoutInMinutes": 60, 
       "buildStatus": "IN_PROGRESS",
       "environment": {
         "computeType": "BUILD_GENERAL1_SMALL",
         "image": "aws/codebuild/standard:2.0",
         "type": "LINUX_CONTAINER",
         "environmentVariables": []
       },
       "source": {
         "type": "S3",
         "location": "codebuild-region-ID-account-ID-input-bucket/MessageUtil.zip"
       },
       "currentPhase": "SUBMITTED",
       "startTime": 1472848787.882,
       "id": "codebuild-demo-project:0cfbb6ec-3db9-4e8c-992b-1ab28EXAMPLE",
       "arn": "arn:aws:codebuild:region-ID:account-ID:build/codebuild-demo-project:0cfbb6ec-3db9-4e8c-992b-1ab28EXAMPLE"    
     }
   }
   ```
   + `build` represents information about this build\.
     + `buildComplete` represents whether the build was completed \(`true`\); otherwise, `false`\.
     + `initiator` represents the entity that started the build\.
     + `artifacts` represents information about the build output, including its location\.
     + `projectName` represents the name of the build project\.
     + `buildStatus` represents the current build status when the start\-build command was run\.
     + `currentPhase` represents the current build phase when the start\-build command was run\.
     + `startTime` represents the time, in Unix time format, when the build process started\.
     + `id` represents the ID of the build\.
     + `arn` represents the ARN of the build\.

   Make a note of the `id` value\. You need it in the next step\.

## Step 7: View Summarized Build Information<a name="getting-started-monitor-build"></a>

In this step, you view summarized information about the status of your build\.

You can use the [AWS CodeBuild console](#getting-started-monitor-build-console) or the [AWS CLI](#getting-started-monitor-build-cli) to complete this step\.

### To view summarized build information \(console\)<a name="getting-started-monitor-build-console"></a>

1. If the **codebuild\-demo\-project:*build\-ID*** page is not displayed, then in the navigation bar, choose **Build history**\. Next, in the list of build projects, for **Project**, choose the **Build run** link for **codebuild\-demo\-project**\. There should be only one matching link\. \(If you have completed this walkthrough before, choose the link in the **Completed** column for the most recent value\.\)

1. On the build details page, in **Phase details**, the following list of build phases should be displayed, with **Succeeded** in the **Status** column:
   + **SUBMITTED**
   + **PROVISIONING**
   + **DOWNLOAD\_SOURCE**
   + **INSTALL**
   + **PRE\_BUILD**
   + **BUILD**
   + **POST\_BUILD**
   + **UPLOAD\_ARTIFACTS**
   + **FINALIZING**
   + **COMPLETED**

   In **Build Status**, **Succeeded** should be displayed\.

   If you see **In Progress** instead, choose the refresh button to see the latest progress\. 

1. Next to each build phase, the **Duration** value indicates how long that build phase lasted\. The **End time** value indicates when that build phase ended\.

   Skip ahead to [Step 8: View Detailed Build Information](#getting-started-build-log)\.

### To view summarized build information \(AWS CLI\)<a name="getting-started-monitor-build-cli"></a>

Use the AWS CLI to run the batch\-get\-builds command\.

```
aws codebuild batch-get-builds --ids id
```

Replace *id* with the `id` value that appeared in the output of the previous step\.

If successful, data similar to this appears in the output\.

```
{
  "buildsNotFound": [],
  "builds": [
    {
      "buildComplete": true,
      "phases": [
        {
          "phaseStatus": "SUCCEEDED",
          "endTime": 1472848788.525,
          "phaseType": "SUBMITTED",
          "durationInSeconds": 0,
          "startTime": 1472848787.882
        },
        ... The full list of build phases has been omitted for brevity ...
        {
          "phaseType": "COMPLETED",
          "startTime": 1472848878.079
        }
      ],
      "logs": {
        "groupName": "/aws/codebuild/codebuild-demo-project",
        "deepLink": "https://console.aws.amazon.com/cloudwatch/home?region=region-ID#logEvent:group=/aws/codebuild/codebuild-demo-project;stream=38ca1c4a-e9ca-4dbc-bef1-d52bfEXAMPLE",
        "streamName": "38ca1c4a-e9ca-4dbc-bef1-d52bfEXAMPLE"
      },
      "artifacts": {
        "md5sum": "MD5-hash",
        "location": "arn:aws:s3:::codebuild-region-ID-account-ID-output-bucket/message-util.zip",
        "sha256sum": "SHA-256-hash"
      },
      "projectName": "codebuild-demo-project",
      "timeoutInMinutes": 60,
      "initiator": "user-name",
      "buildStatus": "SUCCEEDED",
      "environment": {
        "computeType": "BUILD_GENERAL1_SMALL",
        "image": "aws/codebuild/standard:2.0",
        "type": "LINUX_CONTAINER",
        "environmentVariables": []
      },
      "source": {
        "type": "S3",
        "location": "codebuild-region-ID-account-ID-input-bucket/MessageUtil.zip"
      },
      "currentPhase": "COMPLETED",
      "startTime": 1472848787.882,
      "endTime": 1472848878.079,
      "id": "codebuild-demo-project:38ca1c4a-e9ca-4dbc-bef1-d52bfEXAMPLE",
      "arn": "arn:aws:codebuild:region-ID:account-ID:build/codebuild-demo-project:38ca1c4a-e9ca-4dbc-bef1-d52bfEXAMPLE"      
    }
  ]
}
```
+ `buildsNotFound` represents the build IDs for any builds where information is not available\. In this example, it should be empty\.
+ `builds` represents information about each build where information is available\. In this example, information about only one build appears in the output\.
  + `phases` represents the set of build phases CodeBuild runs during the build process\. Information about each build phase is listed separately as `startTime`, `endTime`, and `durationInSeconds` \(when the build phase started and ended, expressed in Unix time format, and how long it lasted, in seconds\), and `phaseType` such as \(`SUBMITTED`, `PROVISIONING`, `DOWNLOAD_SOURCE`, `INSTALL`, `PRE_BUILD`, `BUILD`, `POST_BUILD`, `UPLOAD_ARTIFACTS`, `FINALIZING`, or `COMPLETED`\) and `phaseStatus` \(such as `SUCCEEDED`, `FAILED`, `FAULT`, `TIMED_OUT`, `IN_PROGRESS`, or `STOPPED`\)\. The first time you run the batch\-get\-builds command, there might not be many \(or any\) phases\. After subsequent runs of the batch\-get\-builds command with the same build ID, more build phases should appear in the output\.
  + `logs` represents information in Amazon CloudWatch Logs about the build's logs\.
  + `md5sum` and `sha256sum` represent MD5 and SHA\-256 hashes of the build's output artifact\. These appear in the output only if the build project's `packaging` value is set to `ZIP`\. \(You did not set this value in this walkthrough\.\) You can use these hashes along with a checksum tool to confirm file integrity and authenticity\.
**Note**  
You can also use the Amazon S3 console to view these hashes\. Select the box next to the build output artifact, and then choose **Actions**, **Properties**\. In the **Properties** pane, expand **Metadata**, and view the values for **x\-amz\-meta\-codebuild\-content\-md5** and **x\-amz\-meta\-codebuild\-content\-sha256**\. \(In the Amazon S3 console, the build output artifact's **ETag** value should not be interpreted to be either the MD5 or SHA\-256 hash\.\)  
If you use the AWS SDKs to get these hashes, the values are named `codebuild-content-md5` and `codebuild-content-sha256`\. 
  + `endTime` represents the time, in Unix time format, when the build process ended\.

## Step 8: View Detailed Build Information<a name="getting-started-build-log"></a>

In this step, you view detailed information about your build in CloudWatch Logs\.

You can use the [CodeBuild console](#getting-started-build-log-console) or [AWS CLI](#getting-started-build-log-cli) to complete this step\.<a name="getting-started-build-log-console"></a>

**To view detailed build information \(console\)**

1. With the build details page still displayed from the previous step, the last 10,000 lines of the build log are displayed in **Build logs**\. To see the entire build log in CloudWatch Logs, choose the **View entire log** link\. 

1. In the CloudWatch Logs log stream, you can browse the log events\. By default, only the last set of log events is displayed\. To see earlier log events, scroll to the beginning of the list\.

1. In this walkthrough, most of the log events contain verbose information about CodeBuild downloading and installing build dependency files into its build environment, which you probably don't care about\. You can use the **Filter events** box to reduce the information displayed\. For example, if you enter `"[INFO]"` in the **Filter events** box, only those events that contain `[INFO]` are displayed\. For more information, see [Filter and Pattern Syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/FilterAndPatternSyntax.html) in the *Amazon CloudWatch User Guide*\.

Skip ahead to [Step 9: Get the Build Output Artifact](#getting-started-output)\.<a name="getting-started-build-log-cli"></a>

**To view detailed build information \(AWS CLI\)**

1. Use your web browser to go to the `deepLink` location that appeared in the output in the previous step \(for example, `https://console.aws.amazon.com/cloudwatch/home?region=region-ID#logEvent:group=/aws/codebuild/codebuild-demo-project;stream=38ca1c4a-e9ca-4dbc-bef1-d52bfEXAMPLE`\)\.

1. In the CloudWatch Logs log stream, you can browse the log events\. By default, only the last set of log events is displayed\. To see earlier log events, scroll to the beginning of the list\.

1. In this walkthrough, most of the log events contain verbose information about CodeBuild downloading and installing build dependency files into its build environment, which you probably don't care about\. You can use the **Filter events** box to reduce the information displayed\. For example, if you enter `"[INFO]"` in the **Filter events** box, only those events that contain `[INFO]` are displayed\. For more information, see [Filter and Pattern Syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html) in the *Amazon CloudWatch User Guide*\.

These portions of a CloudWatch Logs log stream pertain to this walkthrough\.

```
...
[Container] 2016/04/15 17:49:42 Entering phase PRE_BUILD 
[Container] 2016/04/15 17:49:42 Running command echo Entering pre_build phase...
[Container] 2016/04/15 17:49:42 Entering pre_build phase... 
[Container] 2016/04/15 17:49:42 Phase complete: PRE_BUILD Success: true 
[Container] 2016/04/15 17:49:42 Entering phase BUILD 
[Container] 2016/04/15 17:49:42 Running command echo Entering build phase... 
[Container] 2016/04/15 17:49:42 Entering build phase...
[Container] 2016/04/15 17:49:42 Running command mvn install 
[Container] 2016/04/15 17:49:44 [INFO] Scanning for projects... 
[Container] 2016/04/15 17:49:44 [INFO]
[Container] 2016/04/15 17:49:44 [INFO] ------------------------------------------------------------------------ 
[Container] 2016/04/15 17:49:44 [INFO] Building Message Utility Java Sample App 1.0 
[Container] 2016/04/15 17:49:44 [INFO] ------------------------------------------------------------------------ 
... 
[Container] 2016/04/15 17:49:55 ------------------------------------------------------- 
[Container] 2016/04/15 17:49:55  T E S T S 
[Container] 2016/04/15 17:49:55 ------------------------------------------------------- 
[Container] 2016/04/15 17:49:55 Running TestMessageUtil 
[Container] 2016/04/15 17:49:55 Inside testSalutationMessage() 
[Container] 2016/04/15 17:49:55 Hi!Robert 
[Container] 2016/04/15 17:49:55 Inside testPrintMessage() 
[Container] 2016/04/15 17:49:55 Robert 
[Container] 2016/04/15 17:49:55 Tests run: 2, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.018 sec
[Container] 2016/04/15 17:49:55  
[Container] 2016/04/15 17:49:55 Results : 
[Container] 2016/04/15 17:49:55  
[Container] 2016/04/15 17:49:55 Tests run: 2, Failures: 0, Errors: 0, Skipped: 0 
...
[Container] 2016/04/15 17:49:56 [INFO] ------------------------------------------------------------------------ 
[Container] 2016/04/15 17:49:56 [INFO] BUILD SUCCESS 
[Container] 2016/04/15 17:49:56 [INFO] ------------------------------------------------------------------------ 
[Container] 2016/04/15 17:49:56 [INFO] Total time: 11.845 s 
[Container] 2016/04/15 17:49:56 [INFO] Finished at: 2016-04-15T17:49:56+00:00 
[Container] 2016/04/15 17:49:56 [INFO] Final Memory: 18M/216M 
[Container] 2016/04/15 17:49:56 [INFO] ------------------------------------------------------------------------ 
[Container] 2016/04/15 17:49:56 Phase complete: BUILD Success: true 
[Container] 2016/04/15 17:49:56 Entering phase POST_BUILD 
[Container] 2016/04/15 17:49:56 Running command echo Entering post_build phase... 
[Container] 2016/04/15 17:49:56 Entering post_build phase... 
[Container] 2016/04/15 17:49:56 Phase complete: POST_BUILD Success: true 
[Container] 2016/04/15 17:49:57 Preparing to copy artifacts 
[Container] 2016/04/15 17:49:57 Assembling file list 
[Container] 2016/04/15 17:49:57 Expanding target/messageUtil-1.0.jar 
[Container] 2016/04/15 17:49:57 Found target/messageUtil-1.0.jar 
[Container] 2016/04/15 17:49:57 Creating zip artifact
```

In this example, CodeBuild successfully completed the pre\-build, build, and post\-build build phases\. It ran the unit tests and successfully built the `messageUtil-1.0.jar` file\.

## Step 9: Get the Build Output Artifact<a name="getting-started-output"></a>

In this step, you get the `messageUtil-1.0.jar` file that CodeBuild built and uploaded to the output bucket\.

You can use the [CodeBuild console](#getting-started-output-console) or [Amazon S3 console](#getting-started-output-s3) to complete this step\.<a name="getting-started-output-console"></a>

**To get the build output artifact \(CodeBuild console\)**

1. With the CodeBuild console still open and the build details page still displayed from the previous step, in **Build Status**, choose the **View artifacts** link\. This opens the folder in Amazon S3 for the build output artifact\. \(If the build details page is not displayed, in the navigation bar, choose **Build history**, and then choose the **Build run** link\.\)

1. Open the folder named `target`, where you find the build output artifact file named `messageUtil-1.0.jar`\.

   Skip ahead to [Step 10: Clean Up](#getting-started-clean-up)\.<a name="getting-started-output-s3"></a>

**To get the build output artifact \(Amazon S3 console\)**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Open the bucket named `codebuild-region-ID-account-ID-output-bucket`\.

1. Open the folder named `codebuild-demo-project`\.

1. Open the folder named `target`, where you find the build output artifact file named `messageUtil-1.0.jar`\.

## Step 10: Clean Up<a name="getting-started-clean-up"></a>

To prevent ongoing charges to your AWS account, you can delete the input bucket used in this walkthrough\. For instructions, see [Deleting or Emptying a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/delete-or-empty-bucket.html) in the *Amazon Simple Storage Service Developer Guide*\.

If you are using the IAM user to delete this bucket instead of an AWS root account or an administrator IAM user, the user must have additional access permissions\. \(Using an AWS root account is not recommended\.\) Add the following statement between the markers \(*\#\#\# BEGIN ADDING STATEMENT HERE \#\#\#* and *\#\#\# END ADDING STATEMENTS HERE \#\#\#*\) to an existing access policy for the user\. Ellipses \(`...`\) are used for brevity\. Do not remove any statements in the existing access policy\. Do not enter these ellipses into the policy\.

```
{
  "Version": "2012-10-17",
  "Id": "...",
  "Statement": [
    ### BEGIN ADDING STATEMENT HERE ###
    {
      "Effect": "Allow",
      "Action": [
        "s3:DeleteBucket",
        "s3:DeleteObject"
      ],
      "Resource": "*"
    }
    ### END ADDING STATEMENT HERE ###
  ]
}
```

## Next Steps<a name="getting-started-next-steps"></a>

In this walkthrough, you used AWS CodeBuild to build a set of Java class files into a JAR file\. You then viewed the build's results\.

You can now try using CodeBuild in your own scenarios by following the instructions in [Plan a Build](planning.md)\. If you don't feel ready yet, you might want to try building some of the samples\. For more information, see [Samples](samples.md)\. 
