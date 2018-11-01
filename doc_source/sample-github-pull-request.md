--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# GitHub Pull Request Sample for AWS CodeBuild<a name="sample-github-pull-request"></a>

AWS CodeBuild now supports webhooks, when the source repository is GitHub\. This means that for an AWS CodeBuild build project that has its source code stored in a private GitHub repository, webhooks enable AWS CodeBuild to rebuild the source code every time a code change is pushed to the private repository\.

## Create a Build Project with GitHub as the Source Repository and Enable Webhooks \(Console\)<a name="sample-github-pull-request-running"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/](https://console.aws.amazon.com/codesuite/codebuild/)\.

1.  If an AWS CodeBuild information page is displayed, choose **Create project**\. Otherwise, on the navigation pane, choose **Build projects**, and then choose **Create build project**\. 

1. On the **Create build project** page, in **Project configuration**, for **Project name**, enter a name for this build project\. Build project names must be unique across each AWS account\. You can also include an optional description of the build project to help other users understand what this project is used for\.

1. In **Source**, for **Source provider**, choose **GitHub**\. Follow the instructions to connect \(or reconnect\) with GitHub and then choose **Authorize**\.

   Expand **Additional configuration**\.

   For **Webhook**, select **Rebuild every time a code change is pushed to this repository**\. You can select this check box only if you chose **Repository in my GitHub account**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/webhook.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. In **Environment**:

   For **Environment image**, do one of the following:
   + To use a Docker image managed by AWS CodeBuild, choose **Managed image**, and then make selections from **Operating system**, **Runtime**, and **Runtime version**\.
   + To use another Docker image, choose **Custom image**\. For **Environment type**, choose **Linux** or **Windows**\. For **Custom image type**, choose **Amazon ECR** or **Other location**\. If you choose **Other location**, enter the name and tag of the Docker image in Docker Hub, using the format `docker repository/docker image name`\. If you choose **Amazon ECR**, then use **Amazon ECR repository** and **Amazon ECR image** to choose the Docker image in your AWS account\. 

1. In **Service role**, do one of the following:
   + If you do not have an AWS CodeBuild service role, choose **New service role**\. In **Role name**, accept the default name or enter your own\.
   + If you have an AWS CodeBuild service role, choose **Existing service role**\. In **Role name**, choose the service role\.
**Note**  
When you use the console to create or update a build project, you can create an AWS CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. For **Buildspec**, do one of the following:
   + Choose **Use a buildspec file** to use the buildspec\.yml file in the source code root directory\.
   + Choose **Insert build commands** to use the console to insert build commands\.

   For more information, see the [Build Spec Reference](build-spec-ref.md)\.

1. In **Artifacts**, for **Type**, do one of the following:
   + If you do not want to create any build output artifacts, choose **No artifacts**\.
   + To store the build output in an Amazon S3 bucket, choose **Amazon S3**, and then do the following:
     + If you want to use your project name for the build output ZIP file or folder, leave **Name** blank\. Otherwise, enter the name\. By default, the artifact name is the project name\. If you want to use a different name, enter it in the artifacts name box\. If you want to output a ZIP file, include the zip extension\.
     + For **Bucket name**, choose the name of the output bucket\.
     + If you chose **Insert build commands** earlier in this procedure, then for **Output files**, enter the locations of the files from the build that you want to put into the build output ZIP file or folder\. For multiple locations, separate each location with a comma \(for example, `appspec.yml, target/my-app.jar`\)\. For more information, see the description of `files` in [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\.

1. Expand **Additional configuration** and set options as appropriate\.

1. Choose **Create build project**\. On the **Review** page, choose **Start build** to run the build\.

## Verification Checks<a name="verification-checks"></a>

1. On your **AWS CodeBuild project** page, choose **Project Details**, and then choose the **Webhook** URL link\.

1. In your GitHub repository, on the **Settings** page, under **Webhooks**, verify that **Pull Request** and **Push** are selected\.

1. In GitHub, under **Accounts**, **Settings**, **Authorized OAuth Apps**, you should see that the AWS CodeBuild region that has been authorized\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/github-oauth-apps.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)