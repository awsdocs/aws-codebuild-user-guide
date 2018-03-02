# GitHub Enterprise Sample for AWS CodeBuild<a name="sample-github-enterprise"></a>

AWS CodeBuild now supports GitHub Enterprise as a source repository\. This sample describes how to set up your AWS CodeBuild projects when your GitHub Enterprise repository has a certificate installed\. It also explains how to enable webhooks so that AWS CodeBuild rebuilds the source code every time a code change is pushed to your private GitHub Enterprise repository\.

## Prerequisites<a name="sample-github-enterprise-prerequisites"></a>

1. Generate a personal access token that will be entered into your AWS CodeBuild project\. We recommend that you create a new GitHub Enterprise user and generate a personal access token for this user\. Copy it to your clipboard so that it can be used when you create your AWS CodeBuild project\. For more information, see [Creating a Personal Access Token in GitHub Enterprise](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) on the GitHub Help website\.

   When you create the personal access token, include the repo scope in the definition\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/scopes.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. Download your certificate from GitHub Enterprise\. AWS CodeBuild uses the certificate to make a trusted SSL connection to the repository\.

   **Linux/macOS clients:**

   From a terminal window, run the following command:

   ```
   echo -n | openssl s_client -connect HOST:PORTNUMBER \
       | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > /folder/filename.pem
   ```

   Replace the placeholders in the command with the following values:

   *HOST*\. The IP address of your GitHub Enterprise repository\.

   *PORTNUMBER*\. The port number you are using to connect \(for example, 443\)\.

   *folder*\. The folder where you downloaded your certificate\.

   *filename*\. The file name of your certificate file\.
**Important**  
Save the certificate as a \.pem file\.

   **Windows clients:**

   Download your certificate from GitHub Enterprise using your browser\. To see the site's certificate details, choose the padlock icon\. For information about how to export the certificate, see your browser documentation\.
**Important**  
Save the certificate as a \.pem file\.

1. Upload your certificate file to an Amazon S3 bucket\. For information about how to create an Amazon S3 bucket, see [How Do I Create an Amazon S3 Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) For information about how to upload objects to an Amazon S3 bucket, see [How Do I Upload Files and Folders to a Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html)
**Note**  
This bucket must be in the same AWS region as your builds\. For example, if you instruct AWS CodeBuild to run a build in the US East \(Ohio\) region, the bucket must be in the US East \(Ohio\) region\.

## Create a Build Project with GitHub Enterprise as the Source Repository and Enable Webhooks \(Console\)<a name="sample-github-enterprise-running"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. If a welcome page is displayed, choose **Get started**\. If a welcome page is not displayed, on the navigation pane, choose **Build projects**, and then choose **Create project**\.

1. On the **Configure your project** page, for **Project name**, type a name for this build project\. Build project names must be unique across each AWS account\.

1. In **Source: What to build**, for **Source provider**, choose **GitHub Enterprise**\.

   + For **Personal Access Token**, paste the token you copied to your clipboard and choose **Save Token**\. In **Repository URL**, enter the URL for your GitHub Enterprise repository\.
**Note**  
You only need to enter and save the personal access token once\. All future AWS CodeBuild projects will use this token\.

   + Select **Webhook** to rebuild every time a code change is pushed to this repository\.

   + Select **Insecure SSL** to ignore SSL warnings while connecting to your GitHub Enterprise project repository\.
**Note**  
We recommend that you use **Insecure SSL** for testing only\. It should not be used in a production environment\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/github-enterprise.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. In **Environment: How to build**:

   For **Environment image**, do one of the following:

   + To use a Docker image managed by AWS CodeBuild, choose **Use an image managed by AWS CodeBuild**, and then make selections from **Operating system**, **Runtime**, and **Version**\.

   + To use another Docker image, choose **Specify a Docker image**\. For **Custom image type**, choose **Other** or **Amazon ECR**\. If you choose **Other**, then for **Custom image ID**, type the name and tag of the Docker image in Docker Hub, using the format `repository-name/image-name:image-tag`\. If you choose **Amazon ECR**, then use **Amazon ECR repository** and **Amazon ECR image** to choose the Docker image in your AWS account\. 

   For **Build specification**, do one of the following:

   + Use the buildspec\.yml file in the source code root directory\.

   + Override the build specification by inserting the build commands\.

   For more information, see the [Build Spec Reference](build-spec-ref.md)\.

   For **Certificate**, choose **Install certificate from your S3**\. For **Bucket of certificate**, choose the S3 bucket where your SSL certificate is stored\. For **Object key of certificate**, type the name of your S3 object key\.

1. In **Artifacts: Where to put the artifacts from this build project**, for **Artifacts type**, do one of the following:

   + If you do not want to create any build output artifacts, choose **No artifacts**\.

   + To store the build output in an Amazon S3 bucket, choose **Amazon S3**, and then do the following:

     + If you want to use your project name for the build output ZIP file or folder, leave **Artifacts name** blank\. Otherwise, type the name in the **Artifacts name** box\. By default, the artifact name is the project name\. If you want to specify a different name, type it in the artifacts name box\. If you want to output a ZIP file, then include the zip extension\.

     + For **Bucket name**, choose the name of the output bucket\.

     + If you chose **Insert build commands** earlier in this procedure, then for **Output files**, type the locations of the files from the build that you want to put into the build output ZIP file or folder\. For multiple locations, separate each location with a comma \(for example, `appspec.yml, target/my-app.jar`\)\. For more information, see the description of `files` in [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\.

1. In **Cache**, do one of the following:

   + If you do not want to use a cache, choose **No cache**\.

   + To use a cache, choose **Amazon S3**, and then do the following:

     + For **Bucket**, choose the name of the Amazon S3 bucket where the cache is stored\.

     + \(Optional\) For **Path prefix**, type an Amazon S3 path prefix\. The **Path prefix** value is similar to a directory name that enables you to store the cache under the same directory in a bucket\. 
**Important**  
Do not append "/" to the end of **Path prefix**\.

   Using a cache saves considerable build time because reusable pieces of the build environment are stored in the cache and used across builds\.

1. In **Service role**, do one of the following:

   + If you do not have an AWS CodeBuild service role, choose **Create a service role in your account**\. In **Role name**, accept the default name or type your own\.

   + If you have an AWS CodeBuild service role, choose **Choose an service existing role from your account**\. In **Role name**, choose the service role\.
**Note**  
When you use the console to create or update a build project, you can create an AWS CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. In **VPC**, do one of the following:

   + If you are not using a VPC for your project, choose **No VPC**\.

   + If you are using want AWS CodeBuild to work with your VPC:

     + For **VPC**, choose the VPC ID that AWS CodeBuild uses\.

     + For **Subnets**, choose the subnets that include resources that AWS CodeBuild uses\.

     + For **Security Groups**, choose the security groups that AWS CodeBuild uses to allow access to resources in the VPCs\.

   For more information, see [Use AWS CodeBuild with Amazon Virtual Private Cloud](vpc-support.md)\.

1. Expand **Show advanced settings** and set them as appropriate\.

1. Choose **Continue**\. On the **Review** page, choose **Save and build** or, to run the build later, choose **Save**\.

1. If you enabled webhooks in **Source: What to Build**, then a **Create webhook** dialog box appears with values displayed for **Payload URL** and **Secret**\. 
**Important**  
The **Create webhook** dialog box appears only once\. Copy the payload URL and secret key\. You need them when you add a webhook in GitHub Enterprise\.   
If you need to generate a payload URL and secret key again, you must first delete the webhook from your GitHub Enterprise repository\. In your AWS CodeBuild project, clear the **Webhook** check box and then choose **Save**\. You can then create or update an AWS CodeBuild project with the **Webhook** check box selected\. The **Create webhook** dialog box appears again\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/webhook-window.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. In GitHub Enterprise, choose the repository where your AWS CodeBuild project is stored, choose **Settings**, choose **Hooks & services **, and then choose **Add webhook**\. Enter the payload URL and secret key, accept the defaults for the other fields, and choose **Add webhook**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/ghe-webhook.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. Return to your AWS CodeBuild project\. Close the **Create webhook** dialog box and choose **Start build**\.