# Bitbucket pull request and webhook filter sample for CodeBuild<a name="sample-bitbucket-pull-request"></a>

AWS CodeBuild supports webhooks when the source repository is Bitbucket\. This means that for a CodeBuild build project that has its source code stored in a Bitbucket repository, webhooks can be used to rebuild the source code every time a code change is pushed to the repository\. For more information, see [Bitbucket webhook events](bitbucket-webhook.md)\. 

This sample shows you how to create a pull request using a Bitbucket repository\. It also shows you how to use a Bitbucket webhook to trigger CodeBuild to create a build of a project\.

**Topics**
+ [Prerequisites](#sample-bitbucket-pull-request-prerequisites)
+ [Create a build project with Bitbucket as the source repository and enable webhooks](#sample-bitbucket-pull-request-create)
+ [Trigger a build with a Bitbucket webhook](#sample-bitbucket-pull-request-trigger)

## Prerequisites<a name="sample-bitbucket-pull-request-prerequisites"></a>

 To run this sample you must connect your AWS CodeBuild project with your Bitbucket account\. 

**Note**  
 CodeBuild has updated its permissions with Bitbucket\. If you previously connected your project to Bitbucket and now receive a Bitbucket connection error, you must reconnect to grant CodeBuild permission to manage your webhooks\. 

## Create a build project with Bitbucket as the source repository and enable webhooks<a name="sample-bitbucket-pull-request-create"></a>

 The following steps describe how to create an AWS CodeBuild project with Bitbucket as a source repository and enable webhooks\. 

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  If a CodeBuild information page is displayed, choose **Create build project**\. Otherwise, on the navigation pane, expand **Build**, choose **Build projects**, and then choose **Create build project**\. 

1. In **Project name**, enter a name for this build project\. Build project names must be unique across each AWS account\. You can also include an optional description of the build project to help other users understand what this project is used for\.

1.  In **Source**, for **Source provider**, choose **Bitbucket**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/bitbucket-pr-sample-source.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

    Follow the instructions to connect or reconnect, and then choose **Grant access**\. 
**Note**  
CodeBuild does not support Bitbucket Server\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/bitbucket-webhook-prerequisite.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  Choose **Use a repository in my account**\. You cannot use a webhook if you use a public Bitbucket repository\. 

1. In **Primary source webhook events**, select **Rebuild every time a code change is pushed to this repository**\. You can select this check box only if you chose **Repository in my Bitbucket account**\.
**Note**  
 If a build is triggered by a Bitbucket webhook, the **Report build status** setting is ignored\. The build status is always sent to Bitbucket\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/github-pr-webhook.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  Choose other settings for your project\. For more information about source provider options and settings, see [Choose source provider](create-project-console.md#create-project-source-provider)\. 

1. Choose **Create build project**\. On the **Review** page, choose **Start build** to run the build\.

## Trigger a build with a Bitbucket webhook<a name="sample-bitbucket-pull-request-trigger"></a>

 For a project that uses Bitbucket webhooks, AWS CodeBuild creates a build when the Bitbucket repository detects a change in your source code\. 

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. On the navigation pane, choose **Build projects**, and then choose a project associated with a Bitbucket repository with webhooks\. For information about creating a Bitbucket webhook project, see [Create a build project with Bitbucket as the source repository and enable webhooks](#sample-bitbucket-pull-request-create)\. 

1.  Make some changes in the code in your project's Bitbucket repository\. 

1.  Create a pull request on your Bitbucket repository\. For more information, see [Making a pull request](https://www.atlassian.com/git/tutorials/making-a-pull-request)\. 

1.  On the Bitbucket webhooks page, choose **View request** to see a list of recent events\. 

1.  Choose **View details** to see details about the response returned by CodeBuild\. It might look something like this: 

   ```
   "response":"Webhook received and buld started: https://us-east-1.console.aws.amazon.com/codebuild/home..."
   "statusCode":200
   ```

1.  Navigate to the Bitbucket pull request page to see the status of the build\. 