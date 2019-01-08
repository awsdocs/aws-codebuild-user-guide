--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Create a Static Website with Build Output Hosted in an Amazon S3 Bucket<a name="sample-disable-artifact-encryption"></a>

 You can disable the encryption of artifacts in a build\. You might want to do this so that you can publish artifacts to a location that is configured to host a website\. \(You cannot publish encrypted artifacts\.\) This sample shows how you can use webhooks to trigger a build and publish its artifacts to an Amazon S3 bucket that is configured to be a website\. 

1.  Follow the instructions in [Setting Up a Static Website](https://docs.aws.amazon.com/AmazonS3/latest/dev//HostingWebsiteOnS3Setup.html) to configure an Amazon S3 bucket to function like a website\. 

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  If an AWS CodeBuild information page is displayed, choose **Create project**\. Otherwise, on the navigation pane, expand **Build**, and then choose **Build projects**\. 

1. On the **Create build project** page, in **Project configuration**, for **Project name**, enter a name for this build project\. Build project names must be unique across each AWS account\. You can also include an optional description of the build project to help other users understand what this project is used for\.

1.  In **Source**, for **Source provider**, choose **GitHub**\. Follow the instructions to connect \(or reconnect\) with GitHub, and then choose **Authorize**\. 

    For **Webhook**, select **Rebuild every time a code change is pushed to this repository**\. You can select this check box only if you chose **Use a repository in my account**\.   
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

1.  In **Artifacts**, for ** Type**, choose **Amazon S3** to store the build output in an Amazon S3 bucket\. 

1.  Select **Disable artifacts encryption**\. 

1.  For **Bucket name**, choose the name of the Amazon S3 bucket you configured to function as a website in step 1\. 

1.  If you chose **Insert build commands** in **Environment**, then for **Output files**, enter the locations of the files from the build that you want to put into the output bucket\. If you have more than one location, use a comma to separate each location \(for example, "appspec\.yml, target/my\-app\.jar"\)\. 

1. Expand **Additional configuration** and set options as appropriate\.

1. Choose **Create build project**\. On the build project page, in **Build history**, choose **Start build** to run the build\.

1.  \(Optional\) Follow the instructions in [Example: Speed Up Your Website with Amazon CloudFront](https://docs.aws.amazon.com/AmazonS3/latest/dev//website-hosting-cloudfront-walkthrough.html)\. 