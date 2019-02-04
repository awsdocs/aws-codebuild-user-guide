--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Bitbucket Pull Request Sample for AWS CodeBuild<a name="sample-bitbucket-pull-request"></a>

This sample shows you how to create a pull request using a Bitbucket repository\. It also shows you how to use a Bitbucket webhook to trigger AWS CodeBuild to create a build of a project\.

**Topics**
+ [Bitbucket Pull Request Prerequisites](#sample-bitbucket-pull-request-prerequisites)
+ [Create a Build Project with Bitbucket as the Source Repository and Enable Webhooks](#sample-bitbucket-pull-request-create)
+ [Trigger a Build with a Bitbucket Webhook](#sample-bitbucket-pull-request-trigger)

## Bitbucket Pull Request Prerequisites<a name="sample-bitbucket-pull-request-prerequisites"></a>

 To run this sample you must connect your AWS CodeBuild project with your Bitbucket account Note, that at this time CodeBuild does not support Enterprise BitBucket integration\. 

**Note**  
 AWS CodeBuild has updated its permissions with Bitbucket\. If you previously connected your project to Bitbucket and now receive a Bitbucket connection error, you must reconnect to grant AWS CodeBuild permission to manage your webhooks\. 

## Create a Build Project with Bitbucket as the Source Repository and Enable Webhooks<a name="sample-bitbucket-pull-request-create"></a>

 The following steps describe how to create an AWS CodeBuild project with Bitbucket as a source repository and enable webhooks\. 

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  If an AWS CodeBuild information page is displayed, choose **Create project**\. Otherwise, on the navigation pane, expand **Build**, and then choose **Build projects**\. 

1. On the **Create build project** page, in **Project configuration**, for **Project name**, enter a name for this build project\. Build project names must be unique across each AWS account\. You can also include an optional description of the build project to help other users understand what this project is used for\.

1.  In **Source**, for **Source provider**, choose **Bitbucket**\. Follow the instructions to connect or reconnect, and then choose **Grant access**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/bitbucket-webhook-prerequisite.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  Choose **Use a repository in my account**\. You cannot use a webhook if you use a public Bitbucket repository\. 

1.  For **Webhook**, select **Rebuild every time a code change is pushed to this repository** 
**Note**  
 If a build is triggered by a Bitbucket webhook, the **Report build status** setting is ignored\. The build status is always sent to Bitbucket\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/source-what-to-build-bitbucket.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  Choose other settings for your project\. For more information about source provider options and settings, see [Choose source provider](create-project.md#create-project-source-provider)\. 

1. Choose **Create build project**\. On the **Review** page, choose **Start build** to run the build\.

## Trigger a Build with a Bitbucket Webhook<a name="sample-bitbucket-pull-request-trigger"></a>

 For a project that uses Bitbucket webhooks, AWS CodeBuild creates a build when the Bitbucket repository detects a change in your source code\. 

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. On the navigation pane, choose **Build projects**, and then choose a project associated with a Bitbucket repository with webhooks\. For information about creating a Bitbucket webhook project, see [Create a Build Project with Bitbucket as the Source Repository and Enable Webhooks](#sample-bitbucket-pull-request-create)\. 

1.  Make some changes in the code in your project's Bitbucket repository\. 

1.  Create a pull request on your Bitbucket repository\. For more information, see [Making a Pull Request](https://www.atlassian.com/git/tutorials/making-a-pull-request)\. 

1.  In the Bitbucket webhooks page, choose **View request** to see a list of recent events\. 

1.  In the Bitbucket webhooks page, choose **View details** to see details about the response returned by AWS CodeBuild\. It might look something like this: 

   ```
   "response":"Webhook received and buld started: https://us-east-1.console.aws.amazon.com/codebuild/home..."
   "statusCode":200
   ```

1.  Navigate to the Bitbucket pull request page to see the status of the build\. 
