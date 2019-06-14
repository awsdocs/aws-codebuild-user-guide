# Key Management<a name="security-key-management"></a>

 You can protect your content from unauthorized use through encryption\. Store your encryption keys in AWS Secrets Manager, and then give CodeBuild permission to obtain the encryption keys from your Secrets Manager account\. For more information, see [Create and Configure an AWS KMS CMK for CodeBuild](setting-up.md#setting-up-kms), [Create a Build Project in CodeBuild](create-project.md), and [Run a Build in CodeBuild](run-build.md)\. 

 Use the `CODEBUILD_KMS_KEY` environment variable in a build command for your AWS KMS key\. For more information, see [Environment Variables in Build Environments](build-env-ref-env-vars.md)\. 

 You can use Secrets Manager to protect credentials to a private registry that stores a Docker image used for your runtime environment\. For more information, see [ Private Registry with AWS Secrets Manager Sample for CodeBuild ](sample-private-registry.md)\. 

## Storing Encryption Keys in AWS Secrets Manager<a name="key-management-store-encryption-keys"></a>

The Secrets Manager secret that stores your encryption keys must be created using the same AWS account that creates the flow\. AWS Elemental MediaConnect does not support cross\-account sharing of secrets\.

**Note**  
You must create the secret in the same AWS Region as your flow\. If you are using two flows to distribute video from one AWS Region to another, you must create two secrets: one secret in each Region\. 

**To store encryption keys in Secrets Manager**

1. Sign in to the AWS Secrets Manager console at [https://console\.aws\.amazon\.com/secretsmanager](https://console.aws.amazon.com/secretsmanager)\.

1. Choose **Store a new secret**\.

1. In **Select secret type**, choose **Other type of secrets**\.

1. In **Specify the key/value pairs to be stored in this secret**, choose **Plaintext**\.

1. Clear any text in the box and replace it with the password value\.

1. Keep **Select the encryption key** set to **DefaultEncryptionKey**\.

1. Choose **Next**\.

1. For **Secret name**, specify a name for your password \(for example, **2018\-12\-01\_baseball\-game\-source**\)\.

1. Choose **Next**\.

1. In **Configure automatic rotation**, choose **Disable automatic rotation**\.

1. Choose **Next**, and then choose **Store**\.

   The details page for your new secret displays information, such as the secret ARN\. You need this value when you create a flow that uses the encryption key that you just stored\.