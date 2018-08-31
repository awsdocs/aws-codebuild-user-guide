# Maven in 5 Minutes Sample for AWS CodeBuild<a name="sample-maven-5m"></a>

This Maven sample produces as build output a single JAR file named `my-app-1.0-SNAPSHOT.jar`\. This sample is based on the [Maven in 5 Minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) topic on the Apache Maven website\.

**Important**  
Running this sample may result in charges to your AWS account\. These include possible charges for AWS CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, and CloudWatch Logs\. For more information, see [AWS CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), and [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

## Running the Sample<a name="sample-maven-5m-running"></a>

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
   ```

   When finished, your directory structure and file should look like this\.

   ```
   (root directory name)
        `-- my-app
              |-- buildspec.yml
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

1. Upload this contents of the `my-app` directory to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\.
**Important**  
Do not upload `(root directory name)` or `(root directory name)/my-app`, just the directories and files inside of `(root directory name)/my-app`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the directory structure and files, and then upload it to the input bucket\. Do not add `(root directory name)` or `(root directory name)/my-app` to the ZIP file, just the directories and files inside of `(root directory name)/my-app`\.

1. Create a build project, run the build, and view related build information by following the steps in [Run AWS CodeBuild Directly](how-to-run.md)\.

   If you use the AWS CLI to create the build project, the JSON\-formatted input to the `create-project` command might look similar to this\. \(Replace the placeholders with your own values\.\)

   ```
   {
     "name": "sample-maven-in-5-minutes-project",
     "source": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-input-bucket/MavenIn5MinutesSample.zip"
     },
     "artifacts": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-output-bucket",
       "packaging": "ZIP",
       "name": "MavenIn5MinutesOutputArtifact.zip"
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

1. Download the `MavenIn5MinutesOutputArtifact.zip` file to your local computer or instance, and then extract the contents of the `MavenIn5MinutesOutputArtifact.zip` file\. In the extracted contents, open the `target` folder to get the `my-app-1.0-SNAPSHOT.jar` file\. 

## Related Resources<a name="w4aab9c48c29b9"></a>
+ For more information about getting started with AWS CodeBuild, see [Getting Started with AWS CodeBuild](getting-started.md)\.
+ For more information about troubleshooting problems with AWS CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.
+ For more information about limits in AWS CodeBuild, see [Limits for AWS CodeBuild](limits.md)\.