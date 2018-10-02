# Create a Static Website with Build Output Hosted in an Amazon S3 Bucket\.<a name="sample-disable-artifact-encryption"></a>

 You can disable the encryption of artifacts in a build\. You might want to do this so that you can publish artifacts to a location that is configured to host a website\. \(You cannot publish encrypted artifacts\.\) This sample shows how you can use webhooks to trigger a build and publish its artifacts to an Amazon S3 bucket that is configured to be a website\. 

1.  Follow the instructions in [Setting Up a Static Website](https://docs.aws.amazon.com/AmazonS3/latest/dev//HostingWebsiteOnS3Setup.html) to configure an Amazon S3 bucket to function like a website\. 

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1.  If a welcome page is displayed, choose **Get started**\. Otherwise, on the navigation pane, choose **Build projects**, and then choose **Create project**\. 

1.  On the **Configure your project** page, for **Project name**, type a name for this build project\. Build project names must be unique across each AWS account\. 

1.  In **Source: What to build**, for **Source provider**, choose **GitHub**\. Follow the instructions to connect \(or reconnect\) with GitHub and choose **Authorize**\. 

    For **Webhook**, select the **Rebuild every time a code change is pushed to this repository** check box\. You can select this check box only if, under** Repository**, you chose **Use a repository in my account**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/webhook.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  In **Environment: How to build**: 

    For **Environment image**, do one of the following: 
   +  To use a Docker image managed by AWS CodeBuild, choose **Use an image managed by AWS CodeBuild**, and then make selections from **Operating system**, **Runtime**, and **Version**\. 
   +  To use another Docker image, choose **Specify a Docker image**\. For **Custom image type**, choose **Other** or **Amazon ECR**\. If you choose **Other**, then for **Custom image ID**, type the name and tag of the Docker image in Docker Hub, using the format `repository-name/image-name:image-tag`\. If you choose **Amazon ECR**, then use **Amazon ECR repository** and **Amazon ECR image** to choose the Docker image in your AWS account\. 

    For **Build specification**, do one of the following: 
   + Use the buildspec\.yml file in the source code root directory\.
   + Override the build specification by inserting build commands\.

   For more information, see the [Build Spec Reference](build-spec-ref.md)\.

1.  In **Artifacts: Where to put the artifacts from this build project**, for ** Type**, choose **Amazon S3** to store the build output in an Amazon S3 bucket\. 

1.  Select **Disable artifacts encryption**\. 

1.  For **Bucket name**, choose the name of the output bucket you created in step 1\. 

1.  If you chose **Insert build commands** in **Environment: How to build**, then for **Output files**, enter the locations of the files from the build that you want to put into the output bucket\. If you have more than one location, use a comma to separate each location \(for example, "appspec\.yml, target/my\-app\.jar"\)\. 

1. In **Service role**, do one of the following:
   + If you do not have an AWS CodeBuild service role, choose **Create a service role in your account**\. In **Role name**, accept the default name or enter your own\.
   + If you have an AWS CodeBuild service role, choose the option to use an existing role\. In **Role name**, choose the service role\.
**Note**  
When you use the console to create or update a build project, you can create an AWS CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. Expand **Show advanced settings** and set them as appropriate\.

1. Choose **Continue**\. On the **Review** page, choose **Save and build** or, to run the build later, choose **Save**\.

1.  \(Optional\) Follow the instructions in [Example: Speed Up Your Website with Amazon CloudFront](https://docs.aws.amazon.com/AmazonS3/latest/dev//website-hosting-cloudfront-walkthrough.html)\. 