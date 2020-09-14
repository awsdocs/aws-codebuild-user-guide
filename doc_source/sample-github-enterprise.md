# GitHub Enterprise Server sample for CodeBuild<a name="sample-github-enterprise"></a>

AWS CodeBuild supports GitHub Enterprise Server as a source repository\. This sample shows how to set up your CodeBuild projects when your GitHub Enterprise Server repository has a certificate installed\. It also shows how to enable webhooks so that CodeBuild rebuilds the source code every time a code change is pushed to your GitHub Enterprise Server repository\.

## Prerequisites<a name="sample-github-enterprise-prerequisites"></a>

1. Generate a personal access token for your CodeBuild project\. We recommend that you create a GitHub Enterprise user and generate a personal access token for this user\. Copy it to your clipboard so that it can be used when you create your CodeBuild project\. For more information, see [Creating a personal access token for the command line](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) on the GitHub Help website\.

   When you create the personal access token, include the **repo** scope in the definition\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/scopes.png)

1. Download your certificate from GitHub Enterprise Server\. CodeBuild uses the certificate to make a trusted SSL connection to the repository\.

   **Linux/macOS clients:**

   From a terminal window, run the following command:

   ```
   echo -n | openssl s_client -connect HOST:PORTNUMBER \
       | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > /folder/filename.pem
   ```

   Replace the placeholders in the command with the following values:

   *HOST*\. The IP address of your GitHub Enterprise Server repository\.

   *PORTNUMBER*\. The port number you are using to connect \(for example, 443\)\.

   *folder*\. The folder where you downloaded your certificate\.

   *filename*\. The file name of your certificate file\.
**Important**  
Save the certificate as a \.pem file\.

   **Windows clients:**

   Use your browser to download your certificate from GitHub Enterprise Server\. To see the site's certificate details, choose the padlock icon\. For information about how to export the certificate, see your browser documentation\.
**Important**  
Save the certificate as a \.pem file\.

1. Upload your certificate file to an S3 bucket\. For information about how to create an S3 bucket, see [How do I create an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) For information about how to upload objects to an S3 bucket, see [How do I upload files and folders to a bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html)
**Note**  
This bucket must be in the same AWS region as your builds\. For example, if you instruct CodeBuild to run a build in the US East \(Ohio\) Region, the bucket must be in the US East \(Ohio\) Region\.

## Create a build project with GitHub Enterprise Server as the source repository and enable webhooks \(console\)<a name="sample-github-enterprise-running"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  If a CodeBuild information page is displayed, choose **Create build project**\. Otherwise, on the navigation pane, expand **Build**, choose **Build projects**, and then choose **Create build project**\. 

1. In **Project name**, enter a name for this build project\. Build project names must be unique across each AWS account\. You can also include an optional description of the build project to help other users understand what this project is used for\.

1. In **Source**, in **Source provider**, choose **GitHub Enterprise**\.
   + For **Personal Access Token**, paste the token you copied to your clipboard and choose **Save Token**\. In **Repository URL**, enter the URL for your GitHub Enterprise Server repository\.
**Note**  
You only need to enter and save the personal access token once\. All future AWS CodeBuild projects use this token\.
   + In **Repository URL**, enter the path to your repository, including the name of the repository\.
   + Expand **Additional configuration**\.
   + Select **Rebuild every time a code change is pushed to this repository** to rebuild every time a code change is pushed to this repository\.
   + Select **Enable insecure SSL** to ignore SSL warnings while you connect to your GitHub Enterprise Server project repository\.
**Note**  
We recommend that you use **Enable insecure SSL** for testing only\. It should not be used in a production environment\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/github-enterprise.png)

1. In **Environment**:

   For **Environment image**, do one of the following:
   + To use a Docker image managed by AWS CodeBuild, choose **Managed image**, and then make selections from **Operating system**, **Runtime\(s\)**, **Image**, and **Image version**\. Make a selection from **Environment type** if it is available\.
   + To use another Docker image, choose **Custom image**\. For **Environment type**, choose **ARM**, **Linux**, **Linux GPU**, or **Windows**\. If you choose **Other registry**, for **External registry URL**, enter the name and tag of the Docker image in Docker Hub, using the format `docker repository/docker image name`\. If you choose **Amazon ECR**, use **Amazon ECR repository** and **Amazon ECR image** to choose the Docker image in your AWS account\.
   + To use a private Docker image, choose **Custom image**\. For **Environment type**, choose **ARM**, **Linux**, **Linux GPU**, or **Windows**\. For **Image registry**, choose **Other registry**, and then enter the ARN of the credentials for your private Docker image\. The credentials must be created by Secrets Manager\. For more information, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/) in the *AWS Secrets Manager User Guide*\.

1. In **Service role**, do one of the following:
   + If you do not have a CodeBuild service role, choose **New service role**\. In **Role name**, enter a name for the new role\.
   + If you have a CodeBuild service role, choose **Existing service role**\. In **Role ARN**, choose the service role\.
**Note**  
When you use the console to create or update a build project, you can create a CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. Expand **Additional configuration**\.

   If you want CodeBuild to work with your VPC:
   + For **VPC**, choose the VPC ID that CodeBuild uses\.
   + For **VPC Subnets**, choose the subnets that include resources that CodeBuild uses\.
   + For **VPC Security groups**, choose the security groups that CodeBuild uses to allow access to resources in the VPCs\.

   For more information, see [Use AWS CodeBuild with Amazon Virtual Private Cloud](vpc-support.md)\.

1. In **Buildspec**, do one of the following:
   + Choose **Use a buildspec file** to use the buildspec\.yml file in the source code root directory\.
   + Choose **Insert build commands** to use the console to insert build commands\.

   For more information, see the [Buildspec reference](build-spec-ref.md)\.

1. In **Artifacts**, for **Type**, do one of the following:
   + If you do not want to create build output artifacts, choose **No artifacts**\.
   + To store the build output in an S3 bucket, choose **Amazon S3**, and then do the following:
     + If you want to use your project name for the build output ZIP file or folder, leave **Name** blank\. Otherwise, enter the name\. By default, the artifact name is the project name\. If you want to use a different name, enter it in the artifacts name box\. If you want to output a ZIP file, include the zip extension\.
     + For **Bucket name**, choose the name of the output bucket\.
     + If you chose **Insert build commands** earlier in this procedure, for **Output files**, enter the locations of the files from the build that you want to put into the build output ZIP file or folder\. For multiple locations, separate each location with a comma \(for example, `appspec.yml, target/my-app.jar`\)\. For more information, see the description of `files` in [Buildspec syntax](build-spec-ref.md#build-spec-ref-syntax)\.

1. For **Cache type**, choose one of the following:
   + If you do not want to use a cache, choose **No cache**\.
   + If you want to use an Amazon S3 cache, choose **Amazon S3**, and then do the following:
     + For **Bucket**, choose the name of the S3 bucket where the cache is stored\.
     + \(Optional\) For **Cache path prefix**, enter an Amazon S3 path prefix\. The **Cache path prefix** value is similar to a directory name\. It makes it possible for you to store the cache under the same directory in a bucket\. 
**Important**  
Do not append a trailing slash \(/\) to the end of the path prefix\.
   +  If you want to use a local cache, choose **Local**, and then choose one or more local cache modes\. 
**Note**  
Docker layer cache mode is available for Linux only\. If you choose it, your project must run in privileged mode\. The `ARM_CONTAINER` and `LINUX_GPU_CONTAINER` environment types and the `BUILD_GENERAL1_2XLARGE` compute type do not support the use of a local cache\.

   Using a cache saves considerable build time because reusable pieces of the build environment are stored in the cache and used across builds\. For information about specifying a cache in the buildspec file, see [Buildspec syntax](build-spec-ref.md#build-spec-ref-syntax)\. For more information about caching, see [Build caching in AWS CodeBuild](build-caching.md)\. 

1. Choose **Create build project**\. On the build project page, choose **Start build**\.

1. If you enabled webhooks in **Source**, a **Create webhook** dialog box is displayed with values for **Payload URL** and **Secret**\. 
**Important**  
The **Create webhook** dialog box appears only once\. Copy the payload URL and secret key\. You need them when you add a webhook in GitHub Enterprise Server\.   
If you need to generate a payload URL and secret key again, you must first delete the webhook from your GitHub Enterprise Server repository\. In your CodeBuild project, clear the **Webhook** check box and then choose **Save**\. You can then create or update a CodeBuild project with the **Webhook** check box selected\. The **Create webhook** dialog box appears again\.

1. In GitHub Enterprise Server, choose the repository where your CodeBuild project is stored\.

1.  Choose **Settings**, choose **Hooks & services**, and then choose **Add webhook**\.

1. Enter the payload URL and secret key, accept the defaults for the other fields, and then choose **Add webhook**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/ghe-webhook.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. Return to your CodeBuild project\. Close the **Create webhook** dialog box and choose **Start build**\.