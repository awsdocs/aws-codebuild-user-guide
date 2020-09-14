# Environment variables in build environments<a name="build-env-ref-env-vars"></a>

AWS CodeBuild provides several environment variables that you can use in your build commands:

AWS\_DEFAULT\_REGION  
The AWS Region where the build is running \(for example, `us-east-1`\)\. This environment variable is used primarily by the AWS CLI\.

AWS\_REGION  
The AWS Region where the build is running \(for example, `us-east-1`\)\. This environment variable is used primarily by the AWS SDKs\.

CODEBUILD\_BATCH\_BUILD\_IDENTIFIER  
The identifier of the build in a batch build\. This is specified in the batch buildspec\. For more information, see [Batch build buildspec reference](batch-build-buildspec.md)\.

CODEBUILD\_BUILD\_ARN  
The Amazon Resource Name \(ARN\) of the build \(for example, `arn:aws:codebuild:region-ID:account-ID:build/codebuild-demo-project:b1e6661e-e4f2-4156-9ab9-82a19EXAMPLE`\)\.

CODEBUILD\_BUILD\_ID  
The CodeBuild ID of the build \(for example, `codebuild-demo-project:b1e6661e-e4f2-4156-9ab9-82a19EXAMPLE`\)\.

CODEBUILD\_BUILD\_IMAGE  
The CodeBuild build image identifier \(for example, `aws/codebuild/standard:2.0`\)\.

CODEBUILD\_BUILD\_NUMBER  
The current build number for the project\.

CODEBUILD\_BUILD\_SUCCEEDING  
Whether the current build is succeeding\. Set to `0` if the build is failing, or `1` if the build is succeeding\.

CODEBUILD\_INITIATOR  
The entity that started the build\. If CodePipeline started the build, this is the pipeline's name \(for example, `codepipeline/my-demo-pipeline`\)\. If an IAM user started the build, this is the user's name \(for example, `MyUserName`\)\. If the Jenkins plugin for CodeBuild started the build, this is the string `CodeBuild-Jenkins-Plugin`\.

CODEBUILD\_KMS\_KEY\_ID  
The identifier of the AWS KMS key that CodeBuild is using to encrypt the build output artifact \(for example, `arn:aws:kms:region-ID:account-ID:key/key-ID` or `alias/key-alias`\)\.

CODEBUILD\_LOG\_PATH  
The log stream name in CloudWatch Logs for the build\.

CODEBUILD\_RESOLVED\_SOURCE\_VERSION  
An identifier for the version of a build's source code\. Its format depends on the source code repository:  
+ For CodeCommit, GitHub, GitHub Enterprise Server, and Bitbucket, it is the commit ID\. For these repositories, `CODEBUILD_RESOLVED_SOURCE_VERSION` is only available after the `DOWNLOAD_SOURCE` phase\. 
+ For CodePipeline, it is the source revision is provided by CodePipeline\. For CodePipeline, the `CODEBUILD_RESOLVED_SOURCE_VERSION` environment variable may not always be available\. 
+ For Amazon S3, this does not apply\. 

CODEBUILD\_SOURCE\_REPO\_URL  
The URL to the input artifact or source code repository\. For Amazon S3, this is `s3://` followed by the bucket name and path to the input artifact\. For CodeCommit and GitHub, this is the repository's clone URL\. If a build originates from CodePipeline, this environment variable may be empty\.  
For secondary sources, the environment variable for the secondary source repository URL is `CODEBUILD_SOURCE_REPO_URL_<sourceIdentifier>`, where `<sourceIdentifier>` is the source identifier you create\. 

CODEBUILD\_SOURCE\_VERSION  
The value's format depends on the source repository\.  
+ For Amazon S3, it is the version ID associated with the input artifact\.
+ For CodeCommit, it is the commit ID or branch name associated with the version of the source code to be built\.
+ For GitHub, GitHub Enterprise Server, and Bitbucket it is the commit ID, branch name, or tag name associated with the version of the source code to be built\.
**Note**  
For a GitHub or GitHub Enterprise Server build that is triggered by a webhook pull request event, it is `pr/pull-request-number`\.
For secondary sources, the environment variable for the secondary source version is `CODEBUILD_SOURCE_VERSION_<sourceIdentifier>`, where `<sourceIdentifier>` is the source identifier you create\. For more information, see [Multiple input sources and output artifacts sample](sample-multi-in-out.md)\.

CODEBUILD\_SRC\_DIR  
The directory path that CodeBuild uses for the build \(for example, `/tmp/src123456789/src`\)\.  
For secondary sources, the environment variable for the secondary source directory path is `CODEBUILD_SRC_DIR_<sourceIdentifier>`, where `<sourceIdentifier>` is the source identifier you create\. For more information, see [Multiple input sources and output artifacts sample](sample-multi-in-out.md)\.

CODEBUILD\_START\_TIME  
The start time of the build specified as a Unix timestamp in milliseconds\.

CODEBUILD\_WEBHOOK\_ACTOR\_ACCOUNT\_ID  
The account ID of the user that triggered the webhook event\.

CODEBUILD\_WEBHOOK\_BASE\_REF  
The base reference name of the webhook event that triggers the current build\. For a pull request, this is the branch reference\.

CODEBUILD\_WEBHOOK\_EVENT  
The webhook event that triggers the current build\.

CODEBUILD\_WEBHOOK\_PREV\_COMMIT  
The ID of the most recent commit before the webhook push event that triggers the current build\.

CODEBUILD\_WEBHOOK\_HEAD\_REF  
The head reference name of the webhook event that triggers the current build\. It can be a branch reference or a tag reference\.

CODEBUILD\_WEBHOOK\_TRIGGER  
Shows the webhook event that triggered the build\. This variable is available only for builds triggered by a webhook\. The value is parsed from the payload sent to CodeBuild by GitHub, GitHub Enterprise Server, or Bitbucket\. The value's format depends on what type of event triggered the build\.  
+ For builds triggered by a pull request, it is `pr/pull-request-number`\. 
+ For builds triggered by creating a new branch or pushing a commit to a branch, it is `branch/branch-name`\. 
+ For builds triggered by a pushing a tag to a repository, it is `tag/tag-name`\. 

HOME  
This environment variable is always set to `/root`\.

You can also provide build environments with your own environment variables\. For more information, see the following topics:
+ [Use CodePipeline with CodeBuild](how-to-create-pipeline.md)
+ [Create a build project](create-project.md)
+ [Change a build project's settings](change-project.md)
+ [Run a build](run-build.md)
+ [Buildspec reference](build-spec-ref.md)

To list all of the available environment variables in a build environment, you can run the `printenv` command \(for Linux\-based build environment\) or `"Get-ChildItem Env:"` \(for Windows\-based build environments\) during a build\. Except for those previously listed, environment variables that start with `CODEBUILD_` are for CodeBuild internal use\. They should not be used in your build commands\.

**Important**  
We strongly discourage the use of environment variables to store sensitive values, especially AWS access key IDs and secret access keys\. Environment variables can be displayed in plain text using tools such as the CodeBuild console and the AWS CLI\.  
We recommend you store sensitive values in the Amazon EC2 Systems Manager Parameter Store and then retrieve them from your buildspec\. To store sensitive values, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Walkthrough: Create and test a String parameter \(console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-console.html) in the *Amazon EC2 Systems Manager User Guide*\. To retrieve them, see the `parameter-store` mapping in [Buildspec syntax](build-spec-ref.md#build-spec-ref-syntax)\.