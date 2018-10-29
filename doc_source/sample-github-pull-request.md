--------

A new console design is available for this service\. Although the procedures in this guide were written for the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\.

--------

# GitHub Pull Request Sample for AWS CodeBuild<a name="sample-github-pull-request"></a>

AWS CodeBuild supports webhooks when the source repository is GitHub\. This means that for an AWS CodeBuild build project that has its source code stored in a private GitHub repository, the use of webhooks enables AWS CodeBuild to begin rebuilding the source code every time a code change is pushed to the private repository\.

## Create a Build Project with GitHub as the Source Repository and Enable Webhooks \(Console\)<a name="sample-github-pull-request-running"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. If a welcome page is displayed, choose **Get started**\. If a welcome page is not displayed, on the navigation pane, choose **Build projects**, and then choose **Create project**\.

1. On the **Configure your project** page, for **Project name**, enter a name for this build project\. Build project names must be unique across each AWS account\.

1. In **Source: What to build**, for **Source provider**, choose **GitHub**\. Follow the instructions to connect \(or reconnect\) with GitHub and choose **Authorize**\.

   For **Webhook**, select **Rebuild every time a code change is pushed to this repository**\. You can select this check box only if you chose **Use a repository in my account**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/webhook.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. In **Environment: How to build**:

   For **Environment image**, do one of the following:
   + To use a Docker image managed by AWS CodeBuild, choose **Use an image managed by AWS CodeBuild**, and then make selections from **Operating system**, **Runtime**, and **Version**\.
   + To use another Docker image, choose **Specify a Docker image**\. For **Custom image type**, choose **Other** or **Amazon ECR**\. If you choose **Other**, then for **Custom image ID**, type the name and tag of the Docker image in Docker Hub, using the format `repository-name/image-name:image-tag`\. If you choose **Amazon ECR**, then use **Amazon ECR repository** and **Amazon ECR image** to choose the Docker image in your AWS account\. 

   For **Build specification**, do one of the following:
   + Use the buildspec\.yml file in the source code root directory\.
   + Override the build specification by inserting the build commands\.

   For more information, see the [Build Spec Reference](build-spec-ref.md)\.

1. In **Artifacts: Where to put the artifacts from this build project**, for **Artifacts type**, do one of the following:
   + If you do not want to create any build output artifacts, choose **No artifacts**\.
   + To store the build output in an Amazon S3 bucket, choose **Amazon S3**, and then do the following:
     + If you want to use your project name for the build output ZIP file or folder, leave **Artifacts name** blank\. Otherwise, type the name in the **Artifacts name** box\. \(By default, the artifact name is the project name\. If you want to specify a different name, type it in the artifacts name box\. If you want to output a ZIP file, then include the zip extension\.
     + For **Bucket name**, choose the name of the output bucket\.
     + If you chose **Insert build commands** earlier in this procedure, then for **Output files**, type the locations of the files from the build that you want to put into the build output ZIP file or folder\. For multiple locations, separate each location with a comma \(for example, `appspec.yml, target/my-app.jar`\)\. For more information, see the description of `files` in [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\.

1. In **Service role**, do one of the following:
   + If you do not have an AWS CodeBuild service role, choose **Create a service role in your account**\. In **Role name**, accept the default name or type your own\.
   + If you have an AWS CodeBuild service role, choose **Choose an service existing role from your account**\. In **Role name**, choose the service role\.
**Note**  
When you use the console to create or update a build project, you can create an AWS CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. Expand **Show advanced settings** and set them as appropriate\.

1. Choose **Continue**\. On the **Review** page, choose **Save and build** or, to run the build later, choose **Save**\.

## Verification Checks<a name="verification-checks"></a>

1. On your **AWS CodeBuild project ** page, do the following:
   + Choose **Project Details** and then choose the **Webhook** URL link\. In your GitHub repository, on the **Settings** page, under **Webhooks**, verify that **Pull Request** and **Push** are both selected\.

1. In GitHub, under **Accounts**, **Settings**, **Authorized OAuth Apps**, you should see the AWS CodeBuild region that has been authorized\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/github-oauth-apps.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)