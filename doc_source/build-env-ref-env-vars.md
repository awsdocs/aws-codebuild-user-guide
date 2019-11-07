# Environment Variables in Build Environments<a name="build-env-ref-env-vars"></a>

AWS CodeBuild provides several environment variables that you can use in your build commands:
+ `AWS_DEFAULT_REGION`: The AWS Region where the build is running \(for example, `us-east-1`\)\. This environment variable is used primarily by the AWS CLI\.
+ `AWS_REGION`: The AWS Region where the build is running \(for example, `us-east-1`\)\. This environment variable is used primarily by the AWS SDKs\.
+ `CODEBUILD_BUILD_ARN`: The Amazon Resource Name \(ARN\) of the build \(for example, `arn:aws:codebuild:region-ID:account-ID:build/codebuild-demo-project:b1e6661e-e4f2-4156-9ab9-82a19EXAMPLE`\)\.
+ `CODEBUILD_BUILD_ID`: The CodeBuild ID of the build \(for example, `codebuild-demo-project:b1e6661e-e4f2-4156-9ab9-82a19EXAMPLE`\)\.
+ `CODEBUILD_BUILD_IMAGE`: The CodeBuild build image identifier \(for example, `aws/codebuild/standard:2.0`\)\.
+ `CODEBUILD_BUILD_NUMBER`: The current build number for the project\.
+ `CODEBUILD_BUILD_SUCCEEDING`: Whether the current build is succeeding\. Set to `0` if the build is failing, or `1` if the build is succeeding\.
+ `CODEBUILD_INITIATOR`: The entity that started the build\. If CodePipeline started the build, this is the pipeline's name \(for example, `codepipeline/my-demo-pipeline`\)\. If an IAM user started the build, this is the user's name \(for example, `MyUserName`\)\. If the Jenkins plugin for CodeBuild started the build, this is the string `CodeBuild-Jenkins-Plugin`\.
+ `CODEBUILD_KMS_KEY_ID`: The identifier of the AWS KMS key that CodeBuild is using to encrypt the build output artifact \(for example, `arn:aws:kms:region-ID:account-ID:key/key-ID` or `alias/key-alias`\)\.
+ `CODEBUILD_LOG_PATH`: The log stream name in CloudWatch Logs for the build\.
+ `CODEBUILD_RESOLVED_SOURCE_VERSION`: An identifier for the version of a build's source code\. Its format depends on the source code repository:
  +  For CodeCommit, GitHub, GitHub Enterprise, and Bitbucket, it is the commit ID\. For these repositories, `CODEBUILD_RESOLVED_SOURCE_VERSION` is only available after the `DOWNLOAD_SOURCE` phase\. 
  +  For CodePipeline, it is the source revision is provided by CodePipeline\. For CodePipeline, the `CODEBUILD_RESOLVED_SOURCE_VERSION` environment variable may not always be available\. 
  +  For Amazon S3, this does not apply\. 
+ `CODEBUILD_SOURCE_REPO_URL`: The URL to the input artifact or source code repository\. For Amazon S3, this is `s3://` followed by the bucket name and path to the input artifact\. For CodeCommit and GitHub, this is the repository's clone URL\. If a build originates from CodePipeline, then this might be empty\.
+ `CODEBUILD_SOURCE_VERSION`: The value's format depends on the source repository\.
  + For Amazon S3, it is the version ID associated with the input artifact\.
  + For CodeCommit, it is the commit ID or branch name associated with the version of the source code to be built\.
  + For GitHub, GitHub Enterprise, and Bitbucket it is the commit ID, branch name, or tag name associated with the version of the source code to be built\.
**Note**  
For a GitHub or GitHub Enterprise build that is triggered by a webhook pull request event, it is `pr/pull-request-number`\.
+ `CODEBUILD_SRC_DIR`: The directory path that CodeBuild uses for the build \(for example, `/tmp/src123456789/src`\)\.
**Note**  
If you use a secondary source, the environment variable for its directory path is `CODEBUILD_SRC_DIR_sourceIdentifier`, where `sourceIdentifier` is the source identifier you create\. For more information, see [Multiple Input Sources and Output Artifacts Sample](sample-multi-in-out.md)\.
+ `CODEBUILD_START_TIME`: The start time of the build specified as a Unix timestamp in milliseconds\.
+ `CODEBUILD_WEBHOOK_ACTOR_ACCOUNT_ID`: The account ID of the user that triggered the webhook event\.
+ `CODEBUILD_WEBHOOK_BASE_REF`: The base reference name of the webhook event that triggers the current build\. For a pull request, this is the branch reference\.
+ `CODEBUILD_WEBHOOK_EVENT`: The webhook event that triggers the current build\.
+ `CODEBUILD_WEBHOOK_PREV_COMMIT`: The ID of the most recent commit before the webhook push event that triggers the current build\.
+ `CODEBUILD_WEBHOOK_HEAD_REF`: The head reference name of the webhook event that triggers the current build\. It can be a branch reference or a tag reference\.
+ `CODEBUILD_WEBHOOK_TRIGGER`: Shows the webhook event that triggered the build\. This variable is available only for builds triggered by a webhook\. The value is parsed from the payload sent to CodeBuild by Github, Github Enterprise, or Bitbucket\. The value's format depends on what type of event triggered the build\.
  +  For builds triggered by a pull request, it is `pr/pull-request-number`\. 
  +  For builds triggered by creating a new branch or pushing a commit to a branch, it is `branch/branch-name`\. 
  +  For builds triggered by a pushing a tag to a repository, it is `tag/tag-name`\. 
+ `HOME`: This environment variable is always set to `/root`\.

You can also provide build environments with your own environment variables\. For more information, see the following topics:
+ [Use AWS CodePipeline with CodeBuild](how-to-create-pipeline.md)
+ [Create a Build Project](create-project.md)
+ [Change a Build Project's Settings](change-project.md)
+ [Run a Build](run-build.md)
+ [Build Spec Reference](build-spec-ref.md)

To list all of the available environment variables in a build environment, you can run the `printenv` command \(for Linux\-based build environment\) or `"Get-ChildItem Env:"` \(for Windows\-based build environments\) during a build\. Except for those previously listed, environment variables that start with `CODEBUILD_` are for CodeBuild internal use\. They should not be used in your build commands\.

**Important**  
We strongly discourage the use of environment variables to store sensitive values, especially AWS access key IDs and secret access keys\. Environment variables can be displayed in plain text using tools such as the CodeBuild console and the AWS CLI\.  
We recommend you store sensitive values in the Amazon EC2 Systems Manager Parameter Store and then retrieve them from your build spec\. To store sensitive values, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\. To retrieve them, see the `parameter-store` mapping in [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\.