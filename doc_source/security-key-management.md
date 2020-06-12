# Key management<a name="security-key-management"></a>

 You can protect your content from unauthorized use through encryption\. Store your encryption keys in AWS Secrets Manager, and then give CodeBuild permission to obtain the encryption keys from your Secrets Manager account\. For more information, see [Create and configure an AWS KMS CMK for CodeBuild](setting-up.md#setting-up-kms), [Create a build project in AWS CodeBuild](create-project.md), [Run a build in AWS CodeBuild](run-build.md), and [Tutorial: Storing and retrieving a secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/tutorials_basic.html)\. 

 Use the `CODEBUILD_KMS_KEY` environment variable in a build command for your AWS KMS key\. For more information, see [Environment variables in build environments](build-env-ref-env-vars.md)\. 

 You can use Secrets Manager to protect credentials to a private registry that stores a Docker image used for your runtime environment\. For more information, see [ Private registry with AWS Secrets Manager sample for CodeBuild](sample-private-registry.md)\. 