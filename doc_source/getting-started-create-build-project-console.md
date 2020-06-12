# Step 5: Create the build project<a name="getting-started-create-build-project-console"></a>

\(Previous step: [Step 4: Upload the source code and the buildspec file](getting-started-upload-source-code-console.md)\)

In this step, you create a build project that AWS CodeBuild uses to run the build\. A *build project* includes information about how to run a build, including where to get the source code, which build environment to use, which build commands to run, and where to store the build output\. A *build environment* represents a combination of operating system, programming language runtime, and tools that CodeBuild uses to run a build\. The build environment is expressed as a Docker image\. For more information, see [Docker overview](https://docs.docker.com/get-started/overview/) on the Docker Docs website\. 

For this build environment, you instruct CodeBuild to use a Docker image that contains a version of the Java Development Kit \(JDK\) and Apache Maven\.<a name="getting-started-create-build-project-console-procedure"></a>

**To create the build project**

1. Sign in to the AWS Management Console and open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. Use the AWS region selector to choose an AWS Region where CodeBuild is supported\. For more information, see [AWS CodeBuild endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the *Amazon Web Services General Reference*\.

1.  If a CodeBuild information page is displayed, choose **Create build project**\. Otherwise, on the navigation pane, expand **Build**, choose **Build projects**, and then choose **Create build project**\. 

1. On the **Create build project** page, in **Project configuration**, for **Project name**, enter a name for this build project \(in this example, `codebuild-demo-project`\)\. Build project names must be unique across each AWS account\. If you use a different name, be sure to use it throughout this tutorial\.
**Note**  
On the **Create build project** page, you might see an error message similar to the following: **You are not authorized to perform this operation\.**\. This is most likely because you signed in to the AWS Management Console as an IAM user who does not have permissions to create a build project\.\. To fix this, sign out of the AWS Management Console, and then sign back in with credentials belonging to one of the following IAM entities:   
An administrator IAM user in your AWS account\. For more information, see [Creating your first IAM admin user and group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
An IAM user in your AWS account with the `AWSCodeBuildAdminAccess`, `AmazonS3ReadOnlyAccess`, and `IAMFullAccess` managed policies attached to that IAM user or to an IAM group that the IAM user belongs to\. If you do not have an IAM user or group in your AWS account with these permissions, and you cannot add these permissions to your IAM user or group, contact your AWS account administrator for assistance\. For more information, see [AWS managed \(predefined\) policies for AWS CodeBuild](auth-and-access-control-iam-identity-based-access-control.md#managed-policies)\.
Both options include administrator permissions that allow you to create a build project so you can complete this tutorial\. We recommend that you always use the minimum permissions required to accomplish your task\. For more information, see [AWS CodeBuild permissions reference](auth-and-access-control-permissions-reference.md)\.

1. In **Source**, for **Source provider**, choose **Amazon S3**\.

1. For **Bucket**, choose **codebuild\-*region\-ID*\-*account\-ID*\-input\-bucket**\.

1.  For **S3 object key**, enter **MessageUtil\.zip**\.

1. In **Environment**, for **Environment image**, leave **Managed image** selected\.

1. For **Operating system**, choose **Amazon Linux 2**\.

1. For **Runtime\(s\)**, choose **Standard**\.

1. For **Image**, choose **aws/codebuild/amazonlinux2\-x86\_64\-standard:3\.0**\.

1. In **Service role**, leave **New service role** selected, and leave **Role name** unchanged\.

1. For **Buildspec**, leave **Use a buildspec file** selected\.

1. In **Artifacts**, for **Type**, choose **Amazon S3**\.

1. For **Bucket name**, choose **codebuild\-*region\-ID*\-*account\-ID*\-output\-bucket**\.

1. Leave **Name** and **Path** blank\.

1. Choose **Create build project**\.

## Next step<a name="getting-started-create-build-project-console-next"></a>

[Step 6: Run the build](getting-started-run-build-console.md)