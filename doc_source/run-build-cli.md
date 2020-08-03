# Run a build \(AWS CLI\)<a name="run-build-cli"></a>

**Note**  
To use CodePipeline to run a build with AWS CodeBuild, skip these steps and follow the instructions in [Create a pipeline that uses CodeBuild \(AWS CLI\)](how-to-create-pipeline.md#how-to-create-pipeline-cli)\.  
For more information about using the AWS CLI with CodeBuild, see the [Command line reference](cmd-ref.md)\.

1. Run the `start-build` command in one of the following ways:

   ```
   aws codebuild start-build --project-name <project-name>
   ```

   Use this if you want to run a build that uses the latest version of the build input artifact and the build project's existing settings\.

   ```
   aws codebuild start-build --generate-cli-skeleton
   ```

   Use this if you want to run a build with an earlier version of the build input artifact or if you want to override the settings for the build output artifacts, environment variables, buildspec, or default build timeout period\.

1. If you run the start\-build command with the `--project-name` option, replace *<project\-name>* with the name of the build project, and then skip to step 6 of this procedure\. To get a list of build projects, see [View a list of build project names](view-project-list.md)\.

1. If you run the start\-build command with the `--idempotency-token` option, a unique case\-sensitive identifier or token, is included with the `start-build` request\. The token is valid for 5 minutes after the request\. If you repeat the `start-build` request with the same token, but change a parameter, CodeBuild returns a parameter mismatch error\.

1. If you run the start\-build command with the `--generate-cli-skeleton` option, JSON\-formatted data appears in the output\. Copy the data to a file \(for example, `start-build.json`\) in a location on the local computer or instance where the AWS CLI is installed\. Modify the copied data to match the following format, and save your results:

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
     + *type*: Optional\. The build output artifact type that overrides for this build the one defined in the build project\.
     + *location*: Optional\. The build output artifact location that overrides for this build the one defined in the build project\.
     + *path*: Optional\. The build output artifact path that overrides for this build the one defined in the build project\.
     + *namespaceType*: Optional\. The build output artifact path type that overrides for this build the one defined in the build project\.
     + *name*: Optional\. The build output artifact name that overrides for this build the one defined in the build project\.
     + *packaging*: Optional\. The build output artifact packaging type that overrides for this build the one defined in the build project\.
   + *buildspecOverride*: Optional\. A buildspec declaration that overrides for this build the one defined in the build project\. If this value is set, it can be either an inline buildspec definition, the path to an alternate buildspec file relative to the value of the built\-in `CODEBUILD_SRC_DIR` environment variable, or the path to an S3 bucket\. The S3 bucket must be in the same AWS Region as the build project\. Specify the buildspec file using its ARN \(for example, `arn:aws:s3:::my-codebuild-sample2/buildspec.yml`\)\. If this value is not provided or is set to an empty string, the source code must contain a `buildspec.yml` file in its root directory\. For more information, see [Buildspec file name and storage location](build-spec-ref.md#build-spec-ref-name-storage)\.
   + The following placeholders are for `cacheOveride`\.
     + *cacheOverride\-location*: Optional\. The location of a `ProjectCache` object for this build that overrides the `ProjectCache` object specified in the build project\. `cacheOverride` is optional and takes a `ProjectCache` object\. `location` is required in a `ProjectCache` object\.
     + *cacheOverride\-type*: Optional\. The type of a `ProjectCache` object for this build that overrides the `ProjectCache` object specified in the build project\. `cacheOverride` is optional and takes a `ProjectCache` object\. `type` is required in a `ProjectCache` object\.
   + *certificateOverride*: Optional\. The name of a certificate for this build that overrides the one specified in the build project\.
   + *environmentTypeOverride*: Optional\. A container type for this build that overrides the one specified in the build project\. The current valid string is `LINUX_CONTAINER`\.
   + The following placeholders are for `environmentVariablesOveride`\.
     + *environmentVariablesOverride\-name*: Optional\. The name of an environment variable in the build project whose value you want to override for this build\.
     + *environmentVariablesOverride\-type*: Optional\. The type of environment variable in the build project whose value you want to override for this build\.
     + *environmentVariablesValue*: Optional\. The value of the environment variable defined in the build project that you want to override for this build\.
   + *gitCloneDepthOverride*: Optional\. The value of the **Git clone depth** in the build project whose value you want to override for this build\. If your source type is Amazon S3, this value is not supported\.
   + *imageOverride*: Optional\. The name of an image for this build that overrides the one specified in the build project\.
   + *idempotencyToken*: Optional\. A string that serves as a token to specify that the build request is idempotent\. You can choose any string that is 64 characters or less\. The token is valid for 5 minutes after the start\-build request\. If you repeat the start\-build request with the same token, but change a parameter, CodeBuild returns a parameter mismatch error\. 
   + *insecureSslOverride*: Optional boolean that specifies whether to override the insecure TLS setting specified in the build project\. The insecure TLS setting determines whether to ignore TLS warnings while connecting to the project source code\. This override applies only if the build's source is GitHub Enterprise Server\.
   + *privilegedModeOverride*: Optional boolean\. If set to true, the build overrides privileged mode in the build project\.
   +  *queuedTimeoutInMinutesOverride*: Optional integer that specifies the number of minutes a build is allowed to be queued before it times out\. Its minimum value is five minutes and its maximum value is 480 minutes \(eight hours\)\. 
   + *reportBuildStatusOverride*: Optional boolean that specifies whether to send your source provider the status of a build's start and completion\. If you set this with a source provider other than GitHub, GitHub Enterprise Server, or Bitbucket, an invalidInputException is thrown\.
   + *sourceAuthOverride*: Optional string\. An authorization type for this build that overrides the one defined in the build project\. This override applies only if the build project's source is Bitbucket or GitHub\.
   + *sourceLocationOverride*: Optional string\. A location that overrides for this build the source location for the one defined in the build project\.
   + *serviceRoleOverride*: Optional string\. The name of a service role for this build that overrides the one specified in the build project\.
   + *sourceTypeOverride*: Optional string\. A source input type for this build that overrides the source input defined in the build project\. Valid strings are `NO_SOURCE`, `CODECOMMIT`, `CODEPIPELINE`, `GITHUB`, `S3`, `BITBUCKET`, and `GITHUB_ENTERPRISE`\.
   + *timeoutInMinutesOverride*: Optional number\. The number of build timeout minutes that overrides for this build the one defined in the build project\. 

   We recommend that you store an environment variable with a sensitive value, such as an AWS access key ID, an AWS secret access key, or a password as a parameter in Amazon EC2 Systems Manager Parameter Store\. CodeBuild can use a parameter stored in Amazon EC2 Systems Manager Parameter Store only if that parameter's name starts with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. You can use the CodeBuild console to create a parameter in Amazon EC2 Systems Manager\. Choose **Create a parameter**, and then follow the instructions\. \(In that dialog box, for **KMS key**, you can optionally specify the ARN of an AWS KMS key in your account\. Amazon EC2 Systems Manager uses this key to encrypt the parameter's value during storage and decrypt during retrieval\.\) If you use the CodeBuild console to create a parameter, the console starts the parameter with `/CodeBuild/` as it is being stored\. However, if you use the Amazon EC2 Systems Manager Parameter Store console to create a parameter, you must start the parameter's name with `/CodeBuild/`, and you must set **Type** to **Secure String**\. For more information, see [AWS Systems Manager parameter store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html) and [Walkthrough: Create and test a String parameter \(console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-console.html) in the *Amazon EC2 Systems Manager User Guide*\.

   If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store, the build project's service role must allow the `ssm:GetParameters` action\. If you chose **Create a new service role in your account** earlier, then CodeBuild includes this action in the default service role for your build project automatically\. However, if you chose **Choose an existing service role from your account**, then you must include this action in your service role separately\.

   Environment variables you set replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\. 

   Do not set any environment variable with a name that begins with `CODEBUILD_`\. This prefix is reserved for internal use\.

   If an environment variable with the same name is defined in multiple places, the environment variable's value is determined as follows:
   + The value in the start build operation call takes highest precedence\.
   + The value in the build project definition takes next precedence\.
   + The value in the buildspec file declaration takes lowest precedence\.

   For information about valid values for these placeholders, see [Create a build project \(AWS CLI\)](create-project-cli.md)\. For a list of the latest settings for a build project, see [View a build project's details](view-project-details.md)\.

1. Switch to the directory that contains the file you just saved, and run the `start-build` command again\.

   ```
   aws codebuild start-build --cli-input-json file://start-build.json
   ```

1. If successful, data similar to that described in the [To run the build](getting-started-cli-run-build.md#getting-started-run-build-cli) procedure appears in the output\.

To work with detailed information about this build, make a note of the `id` value in the output, and then see [View build details \(AWS CLI\)](view-build-details.md#view-build-details-cli)\.