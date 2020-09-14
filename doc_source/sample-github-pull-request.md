# GitHub pull request and webhook filter sample for CodeBuild<a name="sample-github-pull-request"></a>

AWS CodeBuild supports webhooks when the source repository is GitHub\. This means that for a CodeBuild build project that has its source code stored in a GitHub repository, webhooks can be used to rebuild the source code every time a code change is pushed to the repository\.

**Note**  
We recommend that you use a filter group to specify which GitHub users can trigger a build in a public repository\. This can prevent a user from triggering an unexpected build\. For more information, see [GitHub webhook events](github-webhook.md)\. 

## Create a build project with GitHub as the source repository and enable webhooks \(console\)<a name="sample-github-pull-request-running"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  If a CodeBuild information page is displayed, choose **Create build project**\. Otherwise, on the navigation pane, expand **Build**, choose **Build projects**, and then choose **Create build project**\. 

1. Choose **Create build project**\. 

1. In **Project configuration**:  
**Project name**  
Enter a name for this build project\. Build project names must be unique across each AWS account\. You can also include an optional description of the build project to help other users understand what this project is used for\.

1. In **Source**:  
**Source provider**  
Choose **GitHub**\. Follow the instructions to connect \(or reconnect\) with GitHub and then choose **Authorize**\.  
**Repository**  
Choose **Repository in my GitHub account**\.  
**GitHub repository**  
Enter the URL for your GitHub repository\.

1. In **Primary source webhook events**, select **Rebuild every time a code change is pushed to this repository**\. You can select this check box only if you chose **Repository in my GitHub account** in the previous step\.

1. In **Environment**:  
**Environment image**  
Choose one of the following:    
To use a Docker image managed by AWS CodeBuild:  
Choose **Managed image**, and then make selections from **Operating system**, **Runtime\(s\)**, **Image**, and **Image version**\. Make a selection from **Environment type** if it is available\.  
To use another Docker image:  
Choose **Custom image**\. For **Environment type**, choose **ARM**, **Linux**, **Linux GPU**, or **Windows**\. If you choose **Other registry**, for **External registry URL**, enter the name and tag of the Docker image in Docker Hub, using the format `docker repository/docker image name`\. If you choose **Amazon ECR**, use **Amazon ECR repository** and **Amazon ECR image** to choose the Docker image in your AWS account\.  
To use a private Docker image:  
Choose **Custom image**\. For **Environment type**, choose **ARM**, **Linux**, **Linux GPU**, or **Windows**\. For **Image registry**, choose **Other registry**, and then enter the ARN of the credentials for your private Docker image\. The credentials must be created by Secrets Manager\. For more information, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/) in the *AWS Secrets Manager User Guide*\.  
**Service role**  
Choose one of the following:  
   + If you do not have a CodeBuild service role, choose **New service role**\. In **Role name**, enter a name for the new role\.
   + If you have a CodeBuild service role, choose **Existing service role**\. In **Role ARN**, choose the service role\.
When you use the console to create or update a build project, you can create a CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. In **Buildspec**, do one of the following:
   + Choose **Use a buildspec file** to use the buildspec\.yml file in the source code root directory\.
   + Choose **Insert build commands** to use the console to insert build commands\.

   For more information, see the [Buildspec reference](build-spec-ref.md)\.

1. In **Artifacts**:  
**Type**  
Choose one of the following:  
   + If you do not want to create build output artifacts, choose **No artifacts**\.
   + To store the build output in an S3 bucket, choose **Amazon S3**, and then do the following:
     + If you want to use your project name for the build output ZIP file or folder, leave **Name** blank\. Otherwise, enter the name\. By default, the artifact name is the project name\. If you want to use a different name, enter it in the artifacts name box\. If you want to output a ZIP file, include the zip extension\.
     + For **Bucket name**, choose the name of the output bucket\.
     + If you chose **Insert build commands** earlier in this procedure, for **Output files**, enter the locations of the files from the build that you want to put into the build output ZIP file or folder\. For multiple locations, separate each location with a comma \(for example, `appspec.yml, target/my-app.jar`\)\. For more information, see the description of `files` in [Buildspec syntax](build-spec-ref.md#build-spec-ref-syntax)\.  
**Additional configuration**  
Expand **Additional configuration** and set options as appropriate\.

1. Choose **Create build project**\. On the **Review** page, choose **Start build** to run the build\.

## Verification checks<a name="verification-checks"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the navigation pane, choose **Build projects**\.

1. Do one of the following:
   + Choose the link for the build project with webhooks you want to verify, and then choose **Build details**\.
   + Choose the button next to the build project with webhooks you want to verify, choose **View details**, and then choose the **Build details** tab\.

1. In **Primary source webhook events**, choose the **Webhook** URL link\. 

1. In your GitHub repository, on the **Settings** page, under **Webhooks**, verify that **Pull Requests** and **Pushes** are selected\.

1. In your GitHub profile settings, under **Personal settings**, **Applications**, **Authorized OAuth Apps**, you should see that your application has been authorized to access the AWS Region you selected\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/github-oauth-apps.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)