--------

A new console design is available for this service\. Although the procedures in this guide were written for the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\.

--------

# Run a Build in AWS CodeBuild<a name="run-build"></a>

You can use the AWS CodeBuild console, AWS CLI, or AWS SDKs to run a build in AWS CodeBuild\.

**Topics**
+ [Run a Build \(Console\)](#run-build-console)
+ [Run a Build \(AWS CLI\)](#run-build-cli)
+ [Start Running Builds Automatically \(AWS CLI\)](#run-build-cli-auto-start)
+ [Stop Running Builds Automatically \(AWS CLI\)](#run-build-cli-auto-stop)
+ [Run a Build \(AWS SDKs\)](#run-build-sdks)

## Run a Build \(Console\)<a name="run-build-console"></a>

To use AWS CodePipeline to run a build with AWS CodeBuild, skip these steps and follow the instructions in [Use AWS CodePipeline with AWS CodeBuild](how-to-create-pipeline.md)\.

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. Do one of the following:
   + If you just finished creating a build project, the **Build project: *project\-name*** page should be displayed\. Choose **Start build**\.
   + If you created a build project earlier, in the navigation pane, choose **Build projects**\. Choose the build project, and then choose **Start build**\.

1. On the **Start new build** page, do one of the following:
   + For Amazon S3, for the optional **Source version** value, type the version ID that corresponds to the version of the input artifact you want to build\. If **Source version** is left blank, the latest version will be used\.
   + For AWS CodeCommit, for the optional **Source version** value, for **Branch**, choose the name of the branch that contains the version of the source code you want to build\. For **Source version**, accept the displayed HEAD commit ID or type a different one\. If **Source version** is blank, the default branch's HEAD commit ID is used\. You cannot type a tag name for **Source version**\. To specify a tag, type the tag's commit ID\. Change the value for **Git clone depth**\. This will create a shallow clone with a history truncated to the specified number of commits\. If you want a full clone, choose **Full**\.
   + For GitHub or GitHub Enterprise, for the optional **Source version** value, type a commit ID, a pull request ID, a branch name, or a tag name that corresponds to the version of the source code you want to build\. If you specify a pull request ID, it must use the format `pr/pull-request-ID` \(for example, `pr/25`\)\. If you specify a branch name, the branch's HEAD commit ID is used\. If **Source version** is blank, the default branch's HEAD commit ID is used\. Change the value for **Git clone depth**\. This creates a shallow clone with a history truncated to the specified number of commits\. If you want a full clone, choose **Full**\.
   + For Bitbucket, for the optional **Source version** value, type a commit ID, a branch name, or a tag name that corresponds to the version of the source code you want to build\. If you specify a branch name, the branch's HEAD commit ID is used\. If **Source version** is blank, the default branch's HEAD commit ID is used\. Change the value for **Git clone depth**\. This creates a shallow clone with a history truncated to the specified number of commits\. If you want a full clone, choose **Full**\.
   + To use a different source provider for this build only, choose **Override source**\. For more information about source provider options and settings, see [Choose source provider](create-project.md#create-project-source-provider)\.

1. To override secondary sources for this build only:
   + To remove a secondary source, in **Secondary sources**,choose the **X** in its row\.
   + To add a secondary source, choose **Add source**\. Enter a **Source identifier** and use the table in step 5 in [Create a Build Project \(Console\)](create-project.md#create-project-console) to make selections appropriate for your secondary source provider\.

1. To override secondary source versions for this build only:
   + To remove a secondary source version, in **Secondary source versions**, choose the **X** in its row\.
   + To add a secondary source version, choose **Add row**, and enter values in **Source identifier** and **Source version**\.

1.  Expand **Override build project settings**\. 

    Here you can change settings for this build only\. The settings in this section are optional\. 

    Under **Environment: How to build**, you can: 
   +  Choose **Override image** to select a different image\. 
   +  Select or clear **Privileged** to change the privileged setting\. 
   +  Choose **Override build specification** to select a different build specification\. 
   +  From **Certificate** choose a different setting\.

    Under **Artifacts: Where to put the artifacts from this build project**, you can: 
   + From **Type**, choose a different artifacts type\.
   + In **Name**, type a different output artifact name\. 
   + If you want a name specified in the buildspec file to override any name specified in the console, select **Use the name specified in the buildspec file**\. The name in a buildspec file uses the Shell command language\. For example, you can append a date and time to your artifact name so that it is always unique\. Unique artifact names prevent artifacts from being overwritten\. For more information, see [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\.
   + In **Path**, type a different output artifact path\. 
   + In **Namespace type**, choose a different type\. Choose **Build ID** to insert the build ID into the path of the build output file \(for example, `My-Path/Build-ID/My-Artifact.zip`\)\. Otherwise, choose **None**\. 
   + From **Bucket name** choose a different Amazon S3 bucket for your output artifacts\. 
   +  If you do not want your build artifacts encrypted, select **Disable artifacts encryption**\.
   + Select **Artifacts packaging**, choose **Zip** to put the build artifact files in a compressed file\. To put the build artifact files in the specified Amazon S3 bucket individually \(not compressed\), choose **None**\.
   + To override secondary artifacts for this build only:
     + To remove a secondary artifact, in **Secondary artifacts**, choose the **X** in its row\.
     + To add a secondary artifact, choose **Add artifact**, and then enter the information for your secondary artifact\. For more information, see step 8 in [Create a Build Project \(Console\)](create-project.md#create-project-console)\.

   Under **Cache**, from **Type**, choose a different cache setting\.

   Under **Logs**, you can override your log settings by selecting or clearing **CloudWatch Logs** and **S3 logs**\. 
   +  If you enable **CloudWatch logs**: 
     +  In **Group name**, enter the name of your Amazon CloudWatch Logs group\. 
     +  In **Stream name**, enter your Amazon CloudWatch Logs stream name\. 
   +  If you enable **S3 logs**: 
     +  From **Bucket**, choose the name of the S3 bucket for your logs\. 
     +  In **Path prefix**, enter the prefix for your logs\. 

    Under **Service role**, you can change the service role that AWS CodeBuild uses to call dependent AWS services for you\. Choose **Create a role** to have AWS CodeBuild create a service role for you\.

1. Expand **Show advanced options**\.
   + If you want to change the build timeout for this build only, change the value in **Timeout**\.
   + If you want to change the **Compute type**, choose one of the available options\. 

1. Expand **Environment variables**\. 

   If you want to change the environment variables for this build only, change the values for **Name**, **Value**, and **Type**\. Use **Add row** to add a new environment variable for this build only\. Choose the delete \(**X**\) button next to an environment variable you do not want to use in this build\.

   Others can see an environment variable by using the AWS CodeBuild console and the AWS CLI\. If you have no concerns about the visibility of your environment variable, set the **Name** and **Value** fields, and then set **Type** to **Plaintext**\.

   We recommend that you store an environment variable with a sensitive value, such as an AWS access key ID, an AWS secret access key, or a password as a parameter in Amazon EC2 Systems Manager Parameter Store\. For **Type**, choose **Parameter Store**\. For **Name**, type an identifier for AWS CodeBuild to reference\. For **Value**, type the parameter's name as stored in Amazon EC2 Systems Manager Parameter Store\. Using a parameter named `/CodeBuild/dockerLoginPassword` as an example, for **Type**, choose **Parameter Store**\. For **Name**, type `LOGIN_PASSWORD`\. For **Value**, type `/CodeBuild/dockerLoginPassword`\. 
**Important**  
We recommend that you store parameters in Amazon EC2 Systems Manager Parameter Store with parameter names that start with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. You can use the AWS CodeBuild console to create a parameter in Amazon EC2 Systems Manager\. Choose **Create a parameter**, and then follow the instructions in the dialog box\. \(In that dialog box, for **KMS key**, you can optionally specify the ARN of an AWS KMS key in your account\. Amazon EC2 Systems Manager uses this key to encrypt the parameter's value during storage and decrypt during retrieval\.\) If you use the AWS CodeBuild console to create a parameter, the console starts the parameter with `/CodeBuild/` as it is being stored\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store, the build project's service role must allow the `ssm:GetParameters` action\. If you chose **Create a service role in your account** earlier, then AWS CodeBuild includes this action in the default service role for your build project automatically\. However, if you chose **Choose an existing service role from your account**, then you must include this action in your service role separately\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store with parameter names that do not start with `/CodeBuild/`, and you chose **Create a service role in your account**, then you must update that service role to allow access to parameter names that do not start with `/CodeBuild/`\. This is because that service role allows access only to parameter names that start with `/CodeBuild/`\.  
Any environment variables you set replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.  
Do not set any environment variable with a name that begins with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, its value is determined as follows:  
The value in the start build operation call takes highest precedence\.
The value in the build project definition takes next precedence\.
The value in the build spec declaration takes lowest precedence\.

1. Choose **Start build**\.

   For detailed information about this build, see [View Build Details \(Console\)](view-build-details.md#view-build-details-console)\.

## Run a Build \(AWS CLI\)<a name="run-build-cli"></a>

**Note**  
To use AWS CodePipeline to run a build with AWS CodeBuild, skip these steps and follow the instructions in [Create a Pipeline that Uses AWS CodeBuild \(AWS CLI\)](how-to-create-pipeline.md#how-to-create-pipeline-cli)\.  
For more information about using the AWS CLI with AWS CodeBuild, see the [Command Line Reference](cmd-ref.md)\.

1. Run the `start-build` command in one of the following ways:

   ```
   aws codebuild start-build --project-name project-name
   ```

   Use this if you want to run a build that uses the latest version of the build input artifact and the build project's existing settings\.

   ```
   aws codebuild start-build --generate-cli-skeleton
   ```

   Use this if you want to run a build with an earlier version of the build input artifact or if you want to override the settings for the build output artifacts, environment variables, build spec, or default build timeout period\.

1. If you run the `start-build` command with the `--project-name` option, replace *project\-name* with the name of the build project, and then skip to step 6 of this procedure\. To get a list of build projects, see [View a List of Build Project Names](view-project-list.md)\.

1. If you run the `start-build` command with the `--idempotency-token` option, a unique case sensitive identifier, or token, is included with the `start-build` request\. The token is valid for 12 hours after the request\. If you repeat the `start-build` request with the same token, but change a parameter, AWS CodeBuild returns a parameter mismatch error\.

1. If you run the `start-build` command with the `--generate-cli-skeleton` option, JSON\-formatted data appears in the output\. Copy the data to a file \(for example, `start-build.json`\) in a location on the local computer or instance where the AWS CLI is installed\. Modify the copied data to match the following format, and save your results:

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
         "type": "cacheOverride-type",
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
     "reportBuildStatusOverride": "reportBuildStatusOverride",
     "timeoutInMinutesOverride": timeoutInMinutesOverride",
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
     + For AWS CodeCommit, the commit ID that corresponds to the version of the source code you want to build\. If *sourceVersion* is not specified, the default branch's HEAD commit ID is used\. \(You cannot specify a tag name for *sourceVersion*, but you can specify the tag's commit ID\.\)
     + For GitHub, the commit ID, pull request ID, branch name, or tag name that corresponds to the version of the source code you want to build\. If a pull request ID is specified, it must use the format `pr/pull-request-ID` \(for example, `pr/25`\)\. If a branch name is specified, the branch's HEAD commit ID is used\. If *sourceVersion* is not specified, the default branch's HEAD commit ID is used\. 
     + For Bitbucket, the commit ID, branch name, or tag name that corresponds to the version of the source code you want to build\. If a branch name is specified, the branch's HEAD commit ID is used\. If *sourceVersion* is not specified, the default branch's HEAD commit ID is used\. 
   + The following placeholders are for `artifactsOveride`\.
     + *type*: Optional string\. The build output artifact type that overrides for this build the one defined in the build project\.
     + *location*: Optional string\. The build output artifact location that overrides for this build the one defined in the build project\.
     + *path*: Optional string\. The build output artifact path that overrides for this build the one defined in the build project\.
     + *namespaceType*: Optional string\. The build output artifact path type that overrides for this build the one defined in the build project\.
     + *name*: Optional string\. The build output artifact name that overrides for this build the one defined in the build project\.
     + *packaging*: Optional string\. The build output artifact packaging type that overrides for this build the one defined in the build project\.
   + *buildspecOverride*: Optional string\. A build spec declaration that overrides for this build the one defined in the build project\. If this value is set, it can be either an inline build spec definition or the path to an alternate build spec file relative to the value of the built\-in `CODEBUILD_SRC_DIR` environment variable\.
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
   + *idempotencyToken*: Optional string\. A string that serves as a token to specify that the build request is idempotent\. You can choose any string that is 64 characters or less\. The token is valid for 12 hours after the start\-build request\. If you repeat the start\-build request with the same token, but change a parameter, AWS CodeBuild returns a parameter mismatch error\. 
   + *insecureSslOverride*: Optional boolean that specifies whether to override the insecure SSL setting specified in the build project\. The insecure SSL setting determines whether to ignore SSL warnings while connecting to the project source code\. This override applies only if the build's source is GitHub Enterprise\.
   + *privilegedModeOverride*: Optional boolean\. If set to true, the build overrides privileged mode in the build project\.
   + *reportBuildStatusOverride*: Optional boolean that specifies whether to send your source provider the status of a build's start and completion\. If you set this with a source provider other than GitHub or Bitbucket, an invalidInputException is thrown\.
   + *sourceAuthOverride*: Optional string\. An authorization type for this build that overrides the one defined in the build project\. This override applies only if the build project's source is BitBucket or GitHub\.
   + *sourceLocationOverride*: Optional string\. A location that overrides for this build the source location for the one defined in the build project\.
   + *serviceRoleOverride*: Optional string\. The name of a service role for this build that overrides the one specified in the build project\.
   + *sourceTypeOverride*: Optional string\. A source input type for this build that overrides the source input defined in the build project\. Valid strings are `NO_SOURCE`, `CODECOMMIT`, `CODEPIPELINE`, `GITHUB`, `S3`, `BITBUCKET`, and `GITHUB_ENTERPRISE`\.
   + *timeoutInMinutesOverride*: Optional number\. The number of build timeout minutes that overrides for this build the one defined in the build project\. 
**Important**  
We recommend that you store an environment variable with a sensitive value, such as an AWS access key ID, an AWS secret access key, or a password as a parameter in Amazon EC2 Systems Manager Parameter Store\. AWS CodeBuild can use a parameter stored in Amazon EC2 Systems Manager Parameter Store only if that parameter's name starts with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. You can use the AWS CodeBuild console to create a parameter in Amazon EC2 Systems Manager\. Choose **Create a parameter**, and then follow the instructions in the dialog box\. \(In that dialog box, for **KMS key**, you can optionally specify the ARN of an AWS KMS key in your account\. Amazon EC2 Systems Manager uses this key to encrypt the parameter's value during storage and decrypt during retrieval\.\) If you use the AWS CodeBuild console to create a parameter, the console starts the parameter with `/CodeBuild/` as it is being stored\. However, if you use the Amazon EC2 Systems Manager Parameter Store console to create a parameter, you must start the parameter's name with `/CodeBuild/`, and you must set **Type** to **Secure String**\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store, the build project's service role must allow the `ssm:GetParameters` action\. If you chose **Create a new service role in your account** earlier, then AWS CodeBuild includes this action in the default service role for your build project automatically\. However, if you chose **Choose an existing service role from your account**, then you must include this action in your service role separately\.  
Environment variables you set replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.   
Do not set any environment variable with a name that begins with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, the environment variable's value is determined as follows:  
The value in the start build operation call takes highest precedence\.
The value in the build project definition takes next precedence\.
The value in the build spec declaration takes lowest precedence\.

   For information about valid values for these placeholders, see [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli)\. For a list of the latest settings for a build project, see [View a Build Project's Details](view-project-details.md)\.

1. Switch to the directory that contains the file you just saved, and run the `start-build` command again\.

   ```
   aws codebuild start-build --cli-input-json file://start-build.json
   ```

1. If successful, data similar to that described in the [To run the build \(AWS CLI\)](getting-started.md#getting-started-run-build-cli) procedure appears in the output\.

To work with detailed information about this build, make a note of the `id` value in the output, and then see [View Build Details \(AWS CLI\)](view-build-details.md#view-build-details-cli)\.

## Start Running Builds Automatically \(AWS CLI\)<a name="run-build-cli-auto-start"></a>

If your source code is stored in a GitHub or a GitHub Enterprise repository, you can use GitHub webhooks to have AWS CodeBuild rebuild your source code whenever a code change is pushed to the repository\.

Run the `create-webhook` command as follows:

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

Copy the secret key and payload URL from the output\. You need them to add a webhook in GitHub Enterprise\. In GitHub Enterprise, choose the repository where your AWS CodeBuild project is stored, choose **Settings**, choose **Hooks & services**, and then choose **Add webhook**\. Enter the payload URL and secret key, accept the defaults for the other fields, and then choose **Add webhook**\.

## Stop Running Builds Automatically \(AWS CLI\)<a name="run-build-cli-auto-stop"></a>

If your source code is stored in a GitHub or a GitHub Enterprise repository, you can set up GitHub webhooks to have AWS CodeBuild rebuild your source code whenever a code change is pushed to the respository\. For more information, see [Start Running Builds Automatically \(AWS CLI\)](#run-build-cli-auto-start)\.

If you have enabled this behavior, you can turn it off by running the `delete-webhook` command as follows:

```
aws codebuild delete-webhook --project-name  
```
+ where *project\-name* is the name of the build project that contains the source code to be rebuilt\.

If this command is successful, no information and no errors appear in the output\.

**Note**  
This deletes the webhook from your AWS CodeBuild project only\. You should also delete the webhook from your GitHub or GitHub Enterprise repository\.

## Run a Build \(AWS SDKs\)<a name="run-build-sdks"></a>

To use AWS CodePipeline to run a build with AWS CodeBuild, skip these steps and follow the instructions in [Use AWS CodePipeline with AWS CodeBuild to Test Code and Run Builds](how-to-create-pipeline.md) instead\.

For information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.