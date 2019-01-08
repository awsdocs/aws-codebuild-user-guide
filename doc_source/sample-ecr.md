--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Amazon ECR Sample for AWS CodeBuild<a name="sample-ecr"></a>

This sample uses a Docker image in an Amazon Elastic Container Registry \(Amazon ECR\) image repository to build the [Go Sample](sample-go-hw.md) for AWS CodeBuild\.

**Important**  
Running this sample may result in charges to your AWS account\. These include possible charges for AWS CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, CloudWatch Logs, and Amazon ECR\. For more information, see [AWS CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing), and [Amazon Elastic Container Registry Pricing](http://aws.amazon.com/ecr/pricing)\.

## Running the Sample<a name="sample-ecr-running"></a>

To run this sample:

1. To create and push the Docker image to your image repository in Amazon ECR, complete the steps in the Running the Sample section of the [Docker Sample](sample-docker.md)\.

1. To create and upload the source code to be built, complete steps 1 through 4 of the Running the Sample section of the [Go Sample](sample-go-hw.md)\. 

1. Assign permissions to your image repository in Amazon ECR so that AWS CodeBuild can pull the repository's Docker image into the build environment:

   1. If you are using an IAM user instead of an AWS root account or an administrator IAM user to work with Amazon ECR, add the statement \(between *\#\#\# BEGIN ADDING STATEMENT HERE \#\#\#* and *\#\#\# END ADDING STATEMENT HERE \#\#\#*\) to the user \(or IAM group the user is associated with\)\. \(Using an AWS root account is not recommended\.\) This statement enables access to managing permissions for Amazon ECR repositories\. Ellipses \(`...`\) are used for brevity and to help you locate where to add the statement\. Do not remove any statements, and do not type these ellipses into the policy\. For more information, see [Working with Inline Policies Using the AWS Management Console](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_inline-using.html#AddingPermissions_Console) in the *IAM User Guide*\. 

      ```
      {
        "Statement": [
          ### BEGIN ADDING STATEMENT HERE ###
          {
            "Action": [
              "ecr:GetRepositoryPolicy",
              "ecr:SetRepositoryPolicy"
            ],
            "Resource": "*",
            "Effect": "Allow"
          },
          ### END ADDING STATEMENT HERE ###
          ...
        ],
        "Version": "2012-10-17"
      }
      ```
**Note**  
The IAM entity that modifies this policy must have permission in IAM to modify policies\.

   1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

   1. Choose **Repositories**\.

   1. In the list of repository names, choose the name of the repository you created or selected\.

   1. Choose the **Permissions** tab, choose **Add**, and then create a statement\.

   1. For **Sid**, type an identifier \(for example, **CodeBuildAccess**\)\.

   1. For **Effect**, leave **Allow** selected because you want to allow access to AWS CodeBuild\.

   1. For **Principal**, type **codebuild\.amazonaws\.com**\. Leave **Everybody** cleared because you want to allow access to AWS CodeBuild only\.

   1. Skip the **All IAM entities** list\.

   1. For **Action**, select **Pull only actions**\.

      All of the pull\-only actions \(**ecr:GetDownloadUrlForLayer**, **ecr:BatchGetImage**, and **ecr:BatchCheckLayerAvailability**\) will be selected\. 

   1. Choose **Save all**\.

      This policy will be displayed in **Policy document**\.

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Sid": "CodeBuildAccess",
            "Effect": "Allow",
            "Principal": {
              "Service": "codebuild.amazonaws.com"  
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

## Related Resources<a name="w4aac11c45c10b9"></a>
+ For more information about getting started with AWS CodeBuild, see [Getting Started with AWS CodeBuild](getting-started.md)\.
+ For more information about troubleshooting problems with AWS CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.
+ For more information about limits in AWS CodeBuild, see [Limits for AWS CodeBuild](limits.md)\.