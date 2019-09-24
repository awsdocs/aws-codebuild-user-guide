# Amazon ECR Sample for CodeBuild<a name="sample-ecr"></a>

This sample uses a Docker image in an Amazon Elastic Container Registry \(Amazon ECR\) image repository to build a sample Go project\.

**Important**  
Running this sample may result in charges to your AWS account\. These include possible charges for AWS CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, CloudWatch Logs, and Amazon ECR\. For more information, see [CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing), and [Amazon Elastic Container Registry Pricing](http://aws.amazon.com/ecr/pricing)\.

## Running the Sample<a name="sample-ecr-running"></a>

To run this sample:

1. To create and push the Docker image to your image repository in Amazon ECR, complete the steps in the Running the Sample section of the [Docker Sample](sample-docker.md)\.

1. Create a Go project: 

   1. Create the files as described in the [Go Project Structure](#ecr-sample-go-project-file-structure) and [Go Project Files](#sample-ecr-go-project-files) sections of this topic, and then upload them to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\. 
**Important**  
Do not upload `(root directory name)`, just the files inside of `(root directory name)`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the files, and then upload it to the input bucket\. Do not add `(root directory name)` to the ZIP file, just the files inside of `(root directory name)`\.

   1. Create a build project, run the build, and view related build information by following the steps in [Run AWS CodeBuild Directly](how-to-run.md)\.

      If you use the AWS CLI to create the build project, the JSON\-formatted input to the`create-project` command might look similar to this\. \(Replace the placeholders with your own values\.\)

      ```
      {
        "name": "sample-go-project",
        "source": {
          "type": "S3",
          "location": "codebuild-region-ID-account-ID-input-bucket/GoSample.zip"
        },
        "artifacts": {
          "type": "S3",
          "location": "codebuild-region-ID-account-ID-output-bucket",
          "packaging": "ZIP",
          "name": "GoOutputArtifact.zip"
        },
        "environment": {
          "type": "LINUX_CONTAINER",
          "image": "aws/codebuild/standard:2.0",
          "computeType": "BUILD_GENERAL1_SMALL"
        },
        "serviceRole": "arn:aws:iam::account-ID:role/role-name",
        "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
      }
      ```

   1. To get the build output artifact, open your Amazon S3 output bucket\.

   1. Download the `GoOutputArtifact.zip` file to your local computer or instance, and then extract the contents of the file\. In the extracted contents, get the `hello` file\. 

1.  If one of the following is true, you must add permissions to your image repository in Amazon ECR so that AWS CodeBuild can pull its Docker image into the build environment\. 
   +  Your project uses CodeBuild credentials to pull Amazon ECR images\. This is denoted by a value of `CODEBUILD` in the `imagePullCredentialsType` attribute of your ProjectEnvironment\. 
   +  Your project uses a cross\-account Amazon ECR image\. In this case, your project must use its service role to pull Amazon ECR images\. To enable this behavior, set the `imagePullCredentialsType` attribute of your ProjectEnvironment to `SERVICE_ROLE`\. 

   1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/](https://console.aws.amazon.com/ecr/)\.

   1. In the list of repository names, choose the name of the repository you created or selected\.

   1. From the navigation pane choose **Permissions**, choose **Edit**, and then choose **Add statement**\.

   1. For **Statement name**, enter an identifier \(for example, **CodeBuildAccess**\)\.

   1. For **Effect**, leave **Allow** selected\. This indicates that you want to allow access to another AWS account\.

   1. For **Principal**, do one of the following:
      + If your project uses CodeBuild credentials to pull an Amazon ECR image, in **Service principal** enter `codebuild.amazonaws.com`\. 
      + If your project uses a cross\-account Amazon ECR image, for **AWS account IDs** enter IDs of the AWS accounts that you want to give access\.

   1. Skip the **All IAM entities** list\.

   1. For **Action**, select the pull\-only actions **ecr:GetDownloadUrlForLayer**, **ecr:BatchGetImage**, and **ecr:BatchCheckLayerAvailability**\.

   1. Choose **Save**\.

      This policy is displayed in **Permissions**\. The principal is what you entered for **Principal** in step 3f of this procedure:
      + If your project uses CodeBuild credentials to pull an Amazon ECR image, under **Service principals** is `"codebuild.amazonaws.com"`\.
      + If your project uses a cross\-account Amazon ECR image, under **AWS Account IDs** is the ID of the AWS account that you want to give access\.

        The following sample policy uses a cross\-account Amazon ECR image\.

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Sid": "CodeBuildAccess",
            "Effect": "Allow",
            "Principal": {
              "AWS": "arn:aws:iam::AWS-account-ID:root"  
            },
            "Action": [
              "ecr:GetDownloadUrlForLayer",
              "ecr:BatchGetImage",
              "ecr:BatchCheckLayerAvailability"
            ]
          }
        ]
      }
      ```

1. Create a build project, run the build, and view build information by following the steps in [Run AWS CodeBuild Directly](how-to-run.md)\.

   If you use the AWS CLI to create the build project, the JSON\-formatted input to the `create-project` command might look similar to this\. \(Replace the placeholders with your own values\.\)

   ```
   {
     "name": "amazon-ecr-sample-project",
     "source": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-input-bucket/GoSample.zip"
     },
     "artifacts": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-output-bucket",
       "packaging": "ZIP",
       "name": "GoOutputArtifact.zip"
     },
     "environment": {
       "type": "LINUX_CONTAINER",
       "image": "account-ID.dkr.ecr.region-ID.amazonaws.com/your-Amazon-ECR-repo-name:latest",
       "computeType": "BUILD_GENERAL1_SMALL"
     },
     "serviceRole": "arn:aws:iam::account-ID:role/role-name",
     "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
   }
   ```

1. To get the build output artifact, open your Amazon S3 output bucket\.

1. Download the `GoOutputArtifact.zip` file to your local computer or instance, and then extract the contents of the `GoOutputArtifact.zip` file\. In the extracted contents, get the `hello` file\.

## Go Project Structure<a name="ecr-sample-go-project-file-structure"></a>

This sample assumes this directory structure\.

```
(root directory name)
    |-- buildspec.yml
    `-- hello.go
```

## Go Project Files<a name="sample-ecr-go-project-files"></a>

This sample uses these files\.

`buildspec.yml` \(in `(root directory name)`\)

```
version: 0.2

phases:
  install: 
   runtime-versions: 
     golang: 1.12 
  build:
    commands:
      - echo Build started on `date`
      - echo Compiling the Go code...
      - go build hello.go 
  post_build:
    commands:
      - echo Build completed on `date`
artifacts:
  files:
    - hello
```

`hello.go` \(in `(root directory name)`\)

```
package main
import "fmt"

func main() {
  fmt.Println("hello world")
  fmt.Println("1+1 =", 1+1)
  fmt.Println("7.0/3.0 =", 7.0/3.0)
  fmt.Println(true && false)
  fmt.Println(true || false)
  fmt.Println(!true)
}
```

## Related Resources<a name="w16aac11c41c10c13"></a>
+ For more information about getting started with AWS CodeBuild, see [Getting Started with CodeBuild](getting-started.md)\.
+ For more information about troubleshooting problems with CodeBuild, see [Troubleshooting CodeBuild](troubleshooting.md)\.
+ For more information about limits in CodeBuild, see [Limits for CodeBuild](limits.md)\.