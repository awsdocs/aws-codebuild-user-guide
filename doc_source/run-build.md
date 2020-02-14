# Run a Build in CodeBuild<a name="run-build"></a>

You can use the AWS CodeBuild console, AWS CLI, or AWS SDKs to run a build in CodeBuild\.

**Topics**
+ [Run a Build \(Console\)](#run-build-console)
+ [Run a Build \(AWS CLI\)](#run-build-cli)
+ [Start Running Builds Automatically \(AWS CLI\)](#run-build-cli-auto-start)
+ [Stop Running Builds Automatically \(AWS CLI\)](#run-build-cli-auto-stop)
+ [Run a Build \(AWS SDKs\)](#run-build-sdks)

## Run a Build \(Console\)<a name="run-build-console"></a>

To use AWS CodePipeline to run a build with CodeBuild, skip these steps and follow the instructions in [Use AWS CodePipeline with CodeBuild](how-to-create-pipeline.md)\.

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. Do one of the following:
   + If you just finished creating a build project, the **Build project: *project\-name*** page should be displayed\. Choose **Start build**\.
   + If you created a build project earlier, in the navigation pane, choose **Build projects**\. Choose the build project, and then choose **Start build**\.

1. On the **Start build** page, do one of the following:
   + For Amazon S3, for the optional **Source version** value, type the version ID for the version of the input artifact you want to build\. If **Source version** is left blank, the latest version is used\.
   + For CodeCommit, for **Reference type**, choose **Branch**, **Git tag**, or **Commit ID**\. Next, choose the branch, Git tag, or enter a commit ID to specify the version of you source code\. For more information, see [Source Version Sample with CodeBuild](sample-source-version.md)\. Change the value for **Git clone depth**\. This creates a shallow clone with a history truncated to the specified number of commits\. If you want a full clone, choose **Full**\.
   + For GitHub or GitHub Enterprise, for the optional **Source version** value, enter a commit ID, pull request ID, branch name, or tag name for the version of the source code you want to build\. If you specify a pull request ID, it must use the format `pr/pull-request-ID` \(for example, `pr/25`\)\. If you specify a branch name, the branch's HEAD commit ID is used\. If **Source version** is blank, the default branch's HEAD commit ID is used\. Change the value for **Git clone depth**\. This creates a shallow clone with a history truncated to the specified number of commits\. If you want a full clone, choose **Full**\.
   + For Bitbucket, for the optional **Source version** value, enter a commit ID, branch name, or tag name for the version of the source code you want to build\. If you specify a branch name, the branch's HEAD commit ID is used\. If **Source version** is blank, the default branch's HEAD commit ID is used\. Change the value for **Git clone depth**\. This creates a shallow clone with a history truncated to the specified number of commits\. If you want a full clone, choose **Full**\.
   + To use a different source provider for this build only, choose **Advanced build options**\. For more information about source provider options and settings, see [Choose source provider](create-project.md#create-project-source-provider)\.

1.  Choose **Advanced build overrides**\. 

    Here you can change settings for this build only\. The settings in this section are optional\. 

    Under **Source**, you can: 
   + Choose **Add source** to add a secondary source\.
   + Choose **Remove source** to remove a secondary source\.
   + Use **Source provider** and **Source version** to modify settings for a source\. 

    Under **Environment**, you can: 
   +  Override settings for **Environment image**, **Operating system**, **Runtime**, and **Runtime version**\.
   +  Select or clear **Privileged**\. 
**Note**  
By default, Docker containers do not allow access to any devices\. Privileged mode grants a build project's Docker container access to all devices\. For more information, see [Runtime Privilege and Linux Capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.
   + In **Service role**, you can change the service role that CodeBuild uses to call dependent AWS services for you\. Choose **New service role** to have CodeBuild create a service role for you\.
   +  Choose **Override build specification** to use a different build specification\. 
   + Change the value for **Timeout**\.
   + Change the value for **Compute**\. 
   +  From **Certificate**, choose a different setting\.

    Under **Buildspec**, you can: 
   + Choose **Use a buildspec file** to use a buildspec\.yml file\. By default, CodeBuild looks for a file named `buildspec.yml` in the source code root directory\. If your buildspec file uses a different name or location, enter its path from the source root in **Buildspec name** \(for example, **buildspec\-two\.yml** or **configuration/buildspec\.yml**\. If the buildspec file is in an S3 bucket, it must be in the same AWS Region as your build project\. Specify the buildspec file its ARN \(for example, **arn:aws:s3:::my\-codebuild\-sample2/buildspec\.yml**\)\. 
   + Choose **Insert build commands** to enter commands you want to run during the build phase\.

    Under **Build Artifacts**, you can: 
   + From **Type**, choose a different artifacts type\.
   + In **Name**, enter a different output artifact name\. 
   + If you want a name specified in the buildspec file to override any name specified in the console, select **Enable semantic versioning**\. The name in a buildspec file uses the Shell command language\. For example, you can append a date and time to your artifact name so that it is always unique\. Unique artifact names prevent artifacts from being overwritten\. For more information, see [Buildspec Syntax](build-spec-ref.md#build-spec-ref-syntax)\.
   + In **Path**, enter a different output artifact path\. 
   + In **Namespace type**, choose a different type\. Choose **Build ID** to insert the build ID into the path of the build output file \(for example, `My-Path/Build-ID/My-Artifact.zip`\)\. Otherwise, choose **None**\. 
   + From **Bucket name** choose a different Amazon S3 bucket for your output artifacts\. 
   +  If you do not want your build artifacts encrypted, select **Disable artifacts encryption**\.
   + Select **Artifacts packaging**, and then choose **Zip** to put the build artifact files in a compressed file\. To put the build artifact files in the specified Amazon S3 bucket individually \(not compressed\), choose **None**\.
   + Under **Cache**, from **Type**, choose a different cache setting\.
   + To override secondary artifacts for this build only:
     + To remove a secondary artifact, in **Secondary artifacts**, choose the **X** in its row\.
     + To add a secondary artifact, choose **Add artifact**, and then enter the information for your secondary artifact\. For more information, see step 8 in [Create a Build Project \(Console\)](create-project.md#create-project-console)\.

   Under **Logs**, you can override your log settings by selecting or clearing **CloudWatch Logs** and **S3 logs**\. 
   +  If you enable **CloudWatch logs**: 
     +  In **Group name**, enter the name of your Amazon CloudWatch Logs group\. 
     +  In **Stream name**, enter your Amazon CloudWatch Logs stream name\. 
   +  If you enable **S3 logs**: 
     +  From **Bucket**, choose the name of the S3 bucket for your logs\. 
     +  In **Path prefix**, enter the prefix for your logs\. 

    Under **Service role**, you can change the service role that CodeBuild uses to call dependent AWS services for you\. Choose **Create a role** to have CodeBuild create a service role for you\.

1. Expand **Environment variables override**\. 

   If you want to change the environment variables for this build only, change the values for **Name**, **Value**, and **Type**\. Choose **Add environment variable** to add a new environment variable for this build only\. Choose **Remove environment variable** to remove an environment variable you do not want to use in this build\.

   Others can see an environment variable by using the CodeBuild console and the AWS CLI\. If you have no concerns about the visibility of your environment variable, set the **Name** and **Value** fields, and then set **Type** to **Plaintext**\.

   We recommend that you store an environment variable with a sensitive value, such as an AWS access key ID, an AWS secret access key, or a password as a parameter in Amazon EC2 Systems Manager Parameter Store\. For **Type**, choose **Parameter**\. For **Name**, type an identifier for CodeBuild to reference\. For **Value**, enter the parameter's name as stored in Amazon EC2 Systems Manager Parameter Store\. Using a parameter named `/CodeBuild/dockerLoginPassword` as an example, for **Type**, choose **Parameter**\. For **Name**, enter `LOGIN_PASSWORD`\. For **Value**, enter `/CodeBuild/dockerLoginPassword`\. 
**Important**  
We recommend that you store parameters in Amazon EC2 Systems Manager Parameter Store with parameter names that start with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. You can use the CodeBuild console to create a parameter in Amazon EC2 Systems Manager\. Choose **Create a parameter**, and then follow the instructions\. \(In that dialog box, for **KMS key**, you can optionally specify the ARN of an AWS KMS key in your account\. Amazon EC2 Systems Manager uses this key to encrypt the parameter's value during storage and decrypt during retrieval\.\) If you use the CodeBuild console to create a parameter, the console starts the parameter with `/CodeBuild/` as it is being stored\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store, the build project's service role must allow the `ssm:GetParameters` action\. If you chose **Create a service role in your account** earlier, then CodeBuild includes this action in the default service role for your build project automatically\. However, if you chose **Choose an existing service role from your account**, then you must include this action in your service role separately\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store with parameter names that do not start with `/CodeBuild/`, and you chose **Create a service role in your account**, then you must update that service role to allow access to parameter names that do not start with `/CodeBuild/`\. This is because that service role allows access only to parameter names that start with `/CodeBuild/`\.  
Any environment variables you set replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.  
Do not set any environment variable with a name that begins with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, its value is determined as follows:  
The value in the start build operation call takes highest precedence\.
The value in the build project definition takes next precedence\.
The value in the buildspec declaration takes lowest precedence\.

1. Choose **Start build**\.

   For detailed information about this build, see [View Build Details \(Console\)](view-build-details.md#view-build-details-console)\.

## Run a Build \(AWS CLI\)<a name="run-build-cli"></a>

**Note**  
To use CodePipeline to run a build with AWS CodeBuild, skip these steps and follow the instructions in [Create a Pipeline That Uses CodeBuild \(AWS CLI\)](how-to-create-pipeline.md#how-to-create-pipeline-cli)\.  
For more information about using the AWS CLI with CodeBuild, see the [Command Line Reference](cmd-ref.md)\.

1. Run the `start-build` command in one of the following ways:

   ```
   aws codebuild start-build --project-name project-name
   ```

   Use this if you want to run a build that uses the latest version of the build input artifact and the build project's existing settings\.

   ```
   aws codebuild start-build --generate-cli-skeleton
   ```

   Use this if you want to run a build with an earlier version of the build input artifact or if you want to override the settings for the build output artifacts, environment variables, buildspec, or default build timeout period\.

1. If you run the start\-build command with the `--project-name` option, replace *project\-name* with the name of the build project, and then skip to step 6 of this procedure\. To get a list of build projects, see [View a List of Build Project Names](view-project-list.md)\.

1. If you run the start\-build command with the `--idempotency-token` option, a unique case sensitive identifier or token, is included with the `start-build` request\. The token is valid for 12 hours after the request\. If you repeat the `start-build` request with the same token, but change a parameter, CodeBuild returns a parameter mismatch error\.

1. If you run the start\-buildcommand with the `--generate-cli-skeleton` option, JSON\-formatted data appears in the output\. Copy the data to a file \(for example, `start-build.json`\) in a location on the local computer or instance where the AWS CLI is installed\. Modify the copied data to match the following format, and save your results:

   ```
   {
     "projectName": "projectName",
     "sourceVersion": "sourceVersion",
     "artifactsOverride": {
       "type": "type",
       "location": "location",
       "path": "path",
       "namespaceType": "namespaceType",
       "name": "artifactsOverride-name",
       "packaging": "packaging"    
     },
     "buildspecOverride": "buildspecOverride",
     "cacheOverride": {
         "location": "cacheOverride-location",
         "type": "cacheOverride-type"
    },
     "certificateOverride": "certificateOverride",
     "computeTypeOverride": "computeTypeOverride",
     "environmentTypeOverride": "environmentTypeOverride",
     "environmentVariablesOverride": {
         "name": "environmentVariablesOverride-name",
         "value": "environmentVariablesValue",
         "type": "environmentVariablesOverride-type"
     },  
     "gitCloneDepthOverride": "gitCloneDepthOverride",
     "imageOverride": "imageOverride",
     "idempotencyToken": "idempotencyToken",
     "insecureSslOverride": "insecureSslOverride",
     "privilegedModeOverride": "privilegedModeOverride",
     "queuedTimeoutInMinutesOverride": "queuedTimeoutInMinutesOverride",           
     "reportBuildStatusOverride": "reportBuildStatusOverride",
     "timeoutInMinutesOverride": "timeoutInMinutesOverride",
     "sourceAuthOverride": "sourceAuthOverride",
     "sourceLocationOverride": "sourceLocationOverride",
     "serviceRoleOverride": "serviceRoleOverride",
     "sourceTypeOverride": "sourceTypeOverride"
   }
   ```

   Replace the following placeholders:
   + *projectName*: Required string\. The name of the build project to use for this build\. 
   + *sourceVersion*: Optional string\. A version of the source code to be built, as follows:
     + For Amazon S3, the version ID that corresponds to the version of the input ZIP file you want to build\. If *sourceVersion* is not specified, then the latest version is used\.
     + For CodeCommit, the commit ID that corresponds to the version of the source code you want to build\. If *sourceVersion* is not specified, the default branch's HEAD commit ID is used\. \(You cannot specify a tag name for *sourceVersion*, but you can specify the tag's commit ID\.\)
     + For GitHub, the commit ID, pull request ID, branch name, or tag name that corresponds to the version of the source code you want to build\. If a pull request ID is specified, it must use the format `pr/pull-request-ID` \(for example, `pr/25`\)\. If a branch name is specified, the branch's HEAD commit ID is used\. If *sourceVersion* is not specified, the default branch's HEAD commit ID is used\. 
     + For Bitbucket, the commit ID, branch name, or tag name that corresponds to the version of the source code you want to build\. If a branch name is specified, the branch's HEAD commit ID is used\. If *sourceVersion* is not specified, the default branch's HEAD commit ID is used\. 
   + The following placeholders are for `artifactsOveride`\.
     + *type*: Optional string\. The build output artifact type that overrides for this build the one defined in the build project\.
     + *location*: Optional string\. The build output artifact location that overrides for this build the one defined in the build project\.
     + *path*: Optional string\. The build output artifact path that overrides for this build the one defined in the build project\.
     + *namespaceType*: Optional string\. The build output artifact path type that overrides for this build the one defined in the build project\.
     + *name*: Optional string\. The build output artifact name that overrides for this build the one defined in the build project\.
     + *packaging*: Optional string\. The build output artifact packaging type that overrides for this build the one defined in the build project\.
   + *buildspecOverride*: Optional string\. A build spec declaration that overrides for this build the one defined in the build project\. If this value is set, it can be either an inline buildspec definition, the path to an alternate buildspec file relative to the value of the built\-in `CODEBUILD_SRC_DIR` environment variable, or the path to an S3 bucket\. The S3 bucket must be in the same AWS Region as the build project\. Specify the buildspec file using its ARN \(for example, `arn:aws:s3:::my-codebuild-sample2/buildspec.yml`\)\. If this value is not provided or is set to an empty string, the source code must contain a `buildspec.yml` file in its root directory\. For more information, see [Buildspec File Name and Storage Location](build-spec-ref.md#build-spec-ref-name-storage)\.
   + The following placeholders are for `cacheOveride`\.
     + *cacheOverride\-location*: Optional string\. The location of a `ProjectCache` object for this build that overrides the `ProjectCache` object specified in the build project\. `cacheOverride` is optional and takes a `ProjectCache` object\. `location` is required in a `ProjectCache` object\.
     + *cacheOverride\-type*: Optional string\. The type of a `ProjectCache` object for this build that overrides the `ProjectCache` object specified in the build project\. `cacheOverride` is optional and takes a `ProjectCache` object\. `type` is required in a `ProjectCache` object\.
   + *certificateOverride*: Optional string\. The name of a certificate for this build that overrides the one specified in the build project\.
   + *environmentTypeOverride*: Optional string\. A container type for this build that overrides the one specified in the build project\. The current valid string is `LINUX_CONTAINER`\.
   + The following placeholders are for `environmentVariablesOveride`\.
     + *environmentVariablesOverride\-name*: Optional string\. The name of an environment variable in the build project whose value you want to override for this build\.
     + *environmentVariablesOverride\-type*: Optional string\. The type of environment variable in the build project whose value you want to override for this build\.
     + *environmentVariablesValue*: Optional string\. The value of the environment variable defined in the build project that you want to override for this build\.
   + *gitCloneDepthOverride*: Optional string\. The value of the **Git clone depth** in the build project whose value you want to override for this build\. If your source type is Amazon S3, this value is not supported\.
   + *imageOverride*: Optional string\. The name of an image for this build that overrides the one specified in the build project\.
   + *idempotencyToken*: Optional string\. A string that serves as a token to specify that the build request is idempotent\. You can choose any string that is 64 characters or less\. The token is valid for 12 hours after the start\-build request\. If you repeat the start\-build request with the same token, but change a parameter, CodeBuild returns a parameter mismatch error\. 
   + *insecureSslOverride*: Optional boolean that specifies whether to override the insecure SSL setting specified in the build project\. The insecure SSL setting determines whether to ignore SSL warnings while connecting to the project source code\. This override applies only if the build's source is GitHub Enterprise\.
   + *privilegedModeOverride*: Optional boolean\. If set to true, the build overrides privileged mode in the build project\.
   +  *queuedTimeoutInMinutesOverride*: Optional integer that specifies the number of minutes a build is allowed to be queued before it times out\. Its minimum value is five minutes and its maximum value is 480 minutes \(eight hours\)\. 
   + *reportBuildStatusOverride*: Optional boolean that specifies whether to send your source provider the status of a build's start and completion\. If you set this with a source provider other than GitHub, GitHub Enterprise, or Bitbucket, an invalidInputException is thrown\.
   + *sourceAuthOverride*: Optional string\. An authorization type for this build that overrides the one defined in the build project\. This override applies only if the build project's source is BitBucket or GitHub\.
   + *sourceLocationOverride*: Optional string\. A location that overrides for this build the source location for the one defined in the build project\.
   + *serviceRoleOverride*: Optional string\. The name of a service role for this build that overrides the one specified in the build project\.
   + *sourceTypeOverride*: Optional string\. A source input type for this build that overrides the source input defined in the build project\. Valid strings are `NO_SOURCE`, `CODECOMMIT`, `CODEPIPELINE`, `GITHUB`, `S3`, `BITBUCKET`, and `GITHUB_ENTERPRISE`\.
   + *timeoutInMinutesOverride*: Optional number\. The number of build timeout minutes that overrides for this build the one defined in the build project\. 
**Important**  
We recommend that you store an environment variable with a sensitive value, such as an AWS access key ID, an AWS secret access key, or a password as a parameter in Amazon EC2 Systems Manager Parameter Store\. CodeBuild can use a parameter stored in Amazon EC2 Systems Manager Parameter Store only if that parameter's name starts with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. You can use the CodeBuild console to create a parameter in Amazon EC2 Systems Manager\. Choose **Create a parameter**, and then follow the instructions\. \(In that dialog box, for **KMS key**, you can optionally specify the ARN of an AWS KMS key in your account\. Amazon EC2 Systems Manager uses this key to encrypt the parameter's value during storage and decrypt during retrieval\.\) If you use the CodeBuild console to create a parameter, the console starts the parameter with `/CodeBuild/` as it is being stored\. However, if you use the Amazon EC2 Systems Manager Parameter Store console to create a parameter, you must start the parameter's name with `/CodeBuild/`, and you must set **Type** to **Secure String**\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store, the build project's service role must allow the `ssm:GetParameters` action\. If you chose **Create a new service role in your account** earlier, then CodeBuild includes this action in the default service role for your build project automatically\. However, if you chose **Choose an existing service role from your account**, then you must include this action in your service role separately\.  
Environment variables you set replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.   
Do not set any environment variable with a name that begins with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, the environment variable's value is determined as follows:  
The value in the start build operation call takes highest precedence\.
The value in the build project definition takes next precedence\.
The value in the buildspec file declaration takes lowest precedence\.

   For information about valid values for these placeholders, see [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli)\. For a list of the latest settings for a build project, see [View a Build Project's Details](view-project-details.md)\.

1. Switch to the directory that contains the file you just saved, and run the `start-build` command again\.

   ```
   aws codebuild start-build --cli-input-json file://start-build.json
   ```

1. If successful, data similar to that described in the [To run the build](getting-started-cli-run-build.md#getting-started-run-build-cli) procedure appears in the output\.

To work with detailed information about this build, make a note of the `id` value in the output, and then see [View Build Details \(AWS CLI\)](view-build-details.md#view-build-details-cli)\.

## Start Running Builds Automatically \(AWS CLI\)<a name="run-build-cli-auto-start"></a>

If your source code is stored in a GitHub or a GitHub Enterprise repository, you can use GitHub webhooks to have AWS CodeBuild rebuild your source code whenever a code change is pushed to the repository\.

Run the create\-webhookcommand as follows:

```
aws codebuild create-webhook --project-name 
```
+ where *project\-name* is the name of the build project that contains the source code to be rebuilt\.

For GitHub, information similar to the following appears in the output:

```
{
  "webhook": {
    "url": "url"
  }
}
```
+ where *url* is the URL to the GitHub webhook\.

For GitHub Enterprise, information similar to the following appears in the output:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/create-webhook-ghe.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. Copy the secret key and payload URL from the output\. You need them to add a webhook in GitHub Enterprise\. 

1. In GitHub Enterprise, choose the repository where your CodeBuild project is stored\. Choose **Settings**, choose **Hooks & services**, and then choose **Add webhook**\. 

1. Enter the payload URL and secret key, accept the defaults for the other fields, and then choose **Add webhook**\.

## Stop Running Builds Automatically \(AWS CLI\)<a name="run-build-cli-auto-stop"></a>

If your source code is stored in a GitHub or a GitHub Enterprise repository, you can set up GitHub webhooks to have AWS CodeBuild rebuild your source code whenever a code change is pushed to the repository\. For more information, see [Start Running Builds Automatically \(AWS CLI\)](#run-build-cli-auto-start)\.

If you have enabled this behavior, you can turn it off by running the `delete-webhook` command as follows:

```
aws codebuild delete-webhook --project-name  
```
+ where *project\-name* is the name of the build project that contains the source code to be rebuilt\.

If this command is successful, no information and no errors appear in the output\.

**Note**  
This deletes the webhook from your CodeBuild project only\. You should also delete the webhook from your GitHub or GitHub Enterprise repository\.

## Run a Build \(AWS SDKs\)<a name="run-build-sdks"></a>

To use CodePipeline to run a build with AWS CodeBuild, skip these steps and follow the instructions in [Use CodePipeline with CodeBuild to Test Code and Run Builds](how-to-create-pipeline.md) instead\.

For information about using CodeBuild with the AWS SDKs, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.