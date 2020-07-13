# Step 5: Create the build project<a name="getting-started-cli-create-build-project"></a>

\(Previous step: [Step 4: Upload the source code and the buildspec file](getting-started-cli-upload-source-code.md)\)

In this step, you create a build project that AWS CodeBuild uses to run the build\. A *build project* includes information about how to run a build, including where to get the source code, which build environment to use, which build commands to run, and where to store the build output\. A *build environment* represents a combination of operating system, programming language runtime, and tools that CodeBuild uses to run a build\. The build environment is expressed as a Docker image\. For more information, see [Docker overview](https://docs.docker.com/get-started/overview/) on the Docker Docs website\. 

For this build environment, you instruct CodeBuild to use a Docker image that contains a version of the Java Development Kit \(JDK\) and Apache Maven\.<a name="getting-started-cli-create-build-project-cli"></a>

**To create the build project**

1. Use the AWS CLI to run the create\-project command:

   ```
   aws codebuild create-project --generate-cli-skeleton
   ```

   JSON\-formatted data appears in the output\. Copy the data to a file named `create-project.json` in a location on the local computer or instance where the AWS CLI is installed\. If you choose to use a different file name, be sure to use it throughout this tutorial\.

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
       "image": "aws/codebuild/amazonlinux2-x86_64-standard:3.0",
       "computeType": "BUILD_GENERAL1_SMALL"
     },
     "serviceRole": "serviceIAMRole"
   }
   ```

   Replace *serviceIAMRole* with the Amazon Resource Name \(ARN\) of a CodeBuild service role \(for example, `arn:aws:iam::account-ID:role/role-name`\)\. To create one, see [Create a CodeBuild service role](setting-up.md#setting-up-service-role)\.

   In this data:
   + `name` represents a required identifier for this build project \(in this example, `codebuild-demo-project`\)\. Build project names must be unique across all build projects in your account\. 
   + For `source`, `type` is a required value that represents the source code's repository type \(in this example, `S3` for an Amazon S3 bucket\)\.
   + For `source`, `location` represents the path to the source code \(in this example, the input bucket name followed by the ZIP file name\)\.
   + For `artifacts`, `type` is a required value that represents the build output artifact's repository type \(in this example, `S3` for an Amazon S3 bucket\)\.
   + For `artifacts`, `location` represents the name of the output bucket you created or identified earlier \(in this example, `codebuild-region-ID-account-ID-output-bucket`\)\.
   + For `environment`, `type` is a required value that represents the type of build environment \(`LINUX_CONTAINER` is currently the only allowed value\)\.
   + For `environment`, `image` is a required value that represents the Docker image name and tag combination this build project uses, as specified by the Docker image repository type \(in this example, `aws/codebuild/standard:4.0` for a Docker image in the CodeBuild Docker images repository\)\. `aws/codebuild/standard` is the name of the Docker image\. `1.0` is the tag of the Docker image\. 

     To find more Docker images you can use in your scenarios, see the [Build environment reference](build-env-ref.md)\.
   + For `environment`, `computeType` is a required value that represents the computing resources CodeBuild uses \(in this example, `BUILD_GENERAL1_SMALL`\)\.
**Note**  
Other available values in the original JSON\-formatted data, such as `description`, `buildspec`, `auth` \(including `type` and `resource`\), `path`, `namespaceType`, `name` \(for `artifacts`\), `packaging`, `environmentVariables` \(including `name` and `value`\), `timeoutInMinutes`, `encryptionKey`, and `tags` \(including `key` and `value`\) are optional\. They are not used in this tutorial, so they are not shown here\. For more information, see [Create a build project \(AWS CLI\)](create-project-cli.md)\.

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
         "image": "aws/codebuild/standard:4.0",
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
After you run the create\-project command, an error message similar to the following might be output: **User: *user\-ARN* is not authorized to perform: codebuild:CreateProject**\. This is most likely because you configured the AWS CLI with the credentials of an IAM user who does not have sufficient permissions to use CodeBuild to create build projects\. To fix this, configure the AWS CLI with credentials belonging to one of the following IAM entities:   
An administrator IAM user in your AWS account\. For more information, see [Creating your first IAM admin user and group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
An IAM user in your AWS account with the `AWSCodeBuildAdminAccess`, `AmazonS3ReadOnlyAccess`, and `IAMFullAccess` managed policies attached to that IAM user or to an IAM group that the IAM user belongs to\. If you do not have an IAM user or group in your AWS account with these permissions, and you cannot add these permissions to your IAM user or group, contact your AWS account administrator for assistance\. For more information, see [AWS managed \(predefined\) policies for AWS CodeBuild](auth-and-access-control-iam-identity-based-access-control.md#managed-policies)\.

## Next step<a name="getting-started-cli-create-build-project-next"></a>

[Step 6: Run the build](getting-started-cli-run-build.md)