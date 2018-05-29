# WAR Hello World Sample for AWS CodeBuild<a name="sample-war-hw"></a>

This Maven sample produces as build output a single Web application ARchive \(WAR\) file named `my-web-app.war`\.

**Important**  
Running this sample may result in charges to your AWS account\. These include possible charges for AWS CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, and CloudWatch Logs\. For more information, see [AWS CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), and [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

## Running the Sample<a name="sample-war-hw-running"></a>

To run this sample:

1. Download and install Maven\. For more information, see [Downloading Apache Maven](https://maven.apache.org/download.cgi) and [Installing Apache Maven](https://maven.apache.org/install.html) on the Apache Maven website\.

1. Switch to an empty directory on your local computer or instance, and then run this Maven command\.

   ```
   mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-web-app -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
   ```

   If successful, this directory structure and files will be created\.

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

1. Create a file with this content\. Name the file `buildspec.yml`, and then add it to the `(root directory name)/my-web-app` directory\.

   ```
   version: 0.2
   
   phases:
     post_build:
       commands:
         - echo Build completed on `date`
         - mvn package
   artifacts:
     files:
       - target/my-web-app.war
     discard-paths: yes
   ```

   When finished, your directory structure and file should look like this\.

   ```
   (root directory name)
        `-- my-web-app
              |-- buildspec.yml
              |-- pom.xml
              `-- src    
                    `-- main
                          |-- resources
                          `-- webapp
                                |-- WEB-INF
                                |     `-- web.xml
                                `-- index.jsp
   ```

1. Upload the contents of the `my-web-app` directory to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\.
**Important**  
Do not upload `(root directory name)` or `(root directory name)/my-web-app`, just the directories and files inside of `(root directory name)/my-web-app`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the directory structure and files, and then upload it to the input bucket\. Do not add `(root directory name)` or `(root directory name)/my-web-app` to the ZIP file, just the directories and files inside of `(root directory name)/my-web-app`\. For example, the ZIP file should contain these directories and files:  

   ```
   WebArchiveHelloWorldSample.zip
        |-- buildpsec.yml
        |-- pom.xml
        `-- src    
              `-- main
                    |-- resources
                    `-- webapp
                          |-- WEB-INF
                          |     `-- web.xml
                          `-- index.jsp
   ```

1. Create a build project, run the build, and view related build information by following the steps in [Run AWS CodeBuild Directly](how-to-run.md)\.

   If you use the AWS CLI to create the build project, the JSON\-formatted input to the `create-project` command might look similar to this\. \(Replace the placeholders with your own values\.\)

   ```
   {
     "name": "sample-web-archive-project",
     "source": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-input-bucket/WebArchiveHelloWorldSample.zip"
     },
     "artifacts": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-output-bucket",
       "packaging": "ZIP",
       "name": "WebArchiveHelloWorldOutputArtifact.zip"
     },
     "environment": {
       "type": "LINUX_CONTAINER",
       "image": "aws/codebuild/java:openjdk-8",
       "computeType": "BUILD_GENERAL1_SMALL"
     },
     "serviceRole": "arn:aws:iam::account-ID:role/role-name",
     "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
   }
   ```

1. To get the build output artifact, open your Amazon S3 output bucket\.

1. Download the `WebArchiveHelloWorldOutputArtifact.zip` file to your local computer or instance, and then extract the contents of the file\. In the extracted contents, open the `target` folder to get the `my-web-app.war` file\. 

## Related Resources<a name="w3ab1b9c54c39b9"></a>
+ For more information about getting started with AWS CodeBuild, see [Getting Started with AWS CodeBuild](getting-started.md)\.
+ For more information about troubleshooting problems with AWS CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.
+ For more information about limits in AWS CodeBuild, see [Limits for AWS CodeBuild](limits.md)\.