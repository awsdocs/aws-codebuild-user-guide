# CodeDeploy Sample for CodeBuild<a name="sample-codedeploy"></a>

This sample instructs AWS CodeBuild to use Maven to produce as build output a single JAR file named `my-app-1.0-SNAPSHOT.jar`\. This sample then uses CodeDeploy to deploy the JAR file to an Amazon Linux instance\. \(Alternatively, you can use AWS CodePipeline to automate the use of CodeDeploy to deploy the JAR file to an Amazon Linux instance\.\) This sample is based on the [Maven in 5 Minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) topic on the Apache Maven website\.

**Important**  
Running this sample may result in charges to your AWS account\. These include possible charges for CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, CloudWatch Logs, and Amazon EC2\. For more information, see [CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing), and [Amazon EC2 Pricing](http://aws.amazon.com/ec2/pricing)\.

## Running the Sample<a name="sample-codedeploy-running"></a>

To run this sample:

1. Download and install Maven\. For more information, see [Downloading Apache Maven](https://maven.apache.org/download.cgi) and [Installing Apache Maven](https://maven.apache.org/install.html) on the Apache Maven website\.

1. Switch to an empty directory on your local computer or instance, and then run this Maven command\.

   ```
   mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

   If successful, this directory structure and files will be created\.

   ```
   (root directory name)
        `-- my-app
              |-- pom.xml
              `-- src    
                    |-- main
                    |     `-- java
                    |           `-- com
                    |                 `-- mycompany 
                    |                       `-- app    
                    |                             `-- App.java 
                    `-- test        
                          `-- java 
                                `-- com
                                      `-- mycompany 
                                            `-- app
                                                  `-- AppTest.java
   ```

1. Create a file with this content\. Name the file `buildspec.yml`, and then add it to the `(root directory name)/my-app` directory\.

   ```
   version: 0.2
   
   phases:
     install:
       runtime-versions:
         java: openjdk8
     build:
       commands:
         - echo Build started on `date`
         - mvn test 
     post_build:
       commands:
         - echo Build completed on `date`
         - mvn package
   artifacts:
     files:
       - target/my-app-1.0-SNAPSHOT.jar
       - appspec.yml
     discard-paths: yes
   ```

1. Create a file with this content\. Name the file `appspec.yml`, and then add it to the `(root directory name)/my-app` directory\.

   ```
   version: 0.0
   os: linux
   files:
     - source: ./my-app-1.0-SNAPSHOT.jar
       destination: /tmp
   ```

   When finished, your directory structure and file should look like this\.

   ```
   (root directory name)
        `-- my-app
              |-- buildspec.yml
              |-- appspec.yml
              |-- pom.xml
              `-- src    
                    |-- main
                    |     `-- java
                    |           `-- com
                    |                 `-- mycompany 
                    |                       `-- app    
                    |                             `-- App.java 
                    `-- test        
                          `-- java 
                                `-- com
                                      `-- mycompany 
                                            `-- app
                                                  ` -- AppTest.java
   ```

1. Create a ZIP file that contains the directory structure and files inside of `(root directory name)/my-app`, and then upload the ZIP file to a source code repository type supported by AWS CodeBuild and CodeDeploy, such as an Amazon S3 input bucket or a GitHub or Bitbucket repository\. 
**Important**  
If you want to use CodePipeline to deploy the resulting build output artifact, you cannot upload the source code to a Bitbucket repository\.  
Do not add `(root directory name)` or `(root directory name)/my-app` to the ZIP file, just the directories and files inside of `(root directory name)/my-app`\. The ZIP file should contain these directories and files:  

   ```
   CodeDeploySample.zip
        |--buildspec.yml
        |-- appspec.yml
        |-- pom.xml
        `-- src    
              |-- main
              |     `-- java
              |           `-- com
              |                 `-- mycompany 
              |                       `-- app    
              |                             `-- App.java 
              `-- test        
                    `-- java 
                          `-- com
                                `-- mycompany 
                                      `-- app
                                            ` -- AppTest.java
   ```

1. Create a build project by following the steps in [Create a Build Project](create-project.md)\.

   If you use the AWS CLI to create the build project, the JSON\-formatted input to the `create-project` command might look similar to this\. \(Replace the placeholders with your own values\.\)

   ```
   {
     "name": "sample-codedeploy-project",
     "source": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-input-bucket/CodeDeploySample.zip"
     },
     "artifacts": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-output-bucket",
       "packaging": "ZIP",
       "name": "CodeDeployOutputArtifact.zip"
     },
     "environment": {
       "type": "LINUX_CONTAINER",
       "image": "aws/codebuild/standard:2.0",
       "computeType": "BUILD_GENERAL1_SMALL"
     },
     "serviceRole": "arn:aws:iam::account-ID:role/role-name",
     "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
   }
   ```

1. If you plan to deploy the build output artifact with CodeDeploy, then follow the steps in [Run a Build](run-build.md)\. Otherwise, skip this step\. \(This is because if you plan to deploy the build output artifact with CodePipeline, then CodePipeline will use CodeBuild to run the build automatically\.\)

1. Complete the setup steps for using CodeDeploy, including:
   +  Grant the IAM user access to CodeDeploy and the AWS services and actions CodeDeploy depends on\. For more information, see [Provision an IAM User](https://docs.aws.amazon.com/codedeploy/latest/userguide/getting-started-setup.html#getting-started-user) in the *CodeDeploy User Guide*\.
   +  Create or identify a service role to enable CodeDeploy to identify the instances where it will deploy the build output artifact\. For more information, see [Creating a Service Role for CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-create-service-role.html) in the *CodeDeploy User Guide*\.
   +  Create or identify an IAM instance profile to enable your instances to access the Amazon S3 input bucket or GitHub repository that contains the build output artifact\. For more information, see [Creating an IAM Instance Profile for Your Amazon EC2 Instances](https://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-create-iam-instance-profile.html) in the *CodeDeploy User Guide*\.

1. Create or identify an Amazon Linux instance compatible with CodeDeploy where the build output artifact will be deployed\. For more information, see [Working with Instances for CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-prepare-instances.html) in the *CodeDeploy User Guide*\.

1. Create or identify a CodeDeploy application and deployment group\. For more information, see [Creating an Application with CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-create-application.html) in the *CodeDeploy User Guide*\.

1. Deploy the build output artifact to the instance\.

   To deploy with CodeDeploy, see [Deploying a Revision with CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-deploy-revision.html) in the *CodeDeploy User Guide*\.

   To deploy with CodePipeline, see [Use AWS CodePipeline with CodeBuild](how-to-create-pipeline.md)\.

1. To find the build output artifact after the deployment is complete, sign in to the instance and look in the `/tmp` directory for the file named `my-app-1.0-SNAPSHOT.jar`\.

## Related Resources<a name="w6aac11c41c47b9"></a>
+ For more information about getting started with AWS CodeBuild, see [Getting Started with CodeBuild](getting-started.md)\.
+ For more information about troubleshooting problems with CodeBuild, see [Troubleshooting CodeBuild](troubleshooting.md)\.
+ For more information about limits in CodeBuild, see [Limits for CodeBuild](limits.md)\.