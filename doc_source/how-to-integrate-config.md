# Use AWS Config with CodeBuild sample<a name="how-to-integrate-config"></a>

AWS Config provides an inventory of your AWS resources and a history of configuration changes to these resources\. AWS Config now supports AWS CodeBuild as an AWS resource, which means the service can track your CodeBuild projects\. For more information about AWS Config, see [What is AWS Config?](https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html) in the *AWS Config Developer Guide*\.

You can see the following information about CodeBuild resources on the **Resource Inventory** page in the AWS Config console:
+ A timeline of your CodeBuild configuration changes\.
+ Configuration details for each CodeBuild project\.
+ Relationships with other AWS resources\.
+ A list of changes to your CodeBuild projects\.

The procedures in this topic show you how to set up AWS Config and look up and view CodeBuild projects\.

**Topics**
+ [Prerequisites](#how-to-create-a-build-project)
+ [Set up AWS Config](#setup-config)
+ [Look up AWS CodeBuild projects](#lookup-projects)
+ [Viewing AWS CodeBuild configuration details in the AWS Config console](#viewing-config-details)

## Prerequisites<a name="how-to-create-a-build-project"></a>

Create your AWS CodeBuild project\. For instructions, see [Create a build project](create-project.md)\.

## Set up AWS Config<a name="setup-config"></a>
+ [Setting up AWS Config \(console\)](https://docs.aws.amazon.com/config/latest/developerguide/gs-console.html)
+ [Setting up AWS Config \(AWS CLI\)](https://docs.aws.amazon.com/config/latest/developerguide/gs-cli.html)

**Note**  
After you complete setup, it might take up to 10 minutes before you can see AWS CodeBuild projects in the AWS Config console\.

## Look up AWS CodeBuild projects<a name="lookup-projects"></a>

1. Sign in to the AWS Management Console and open the AWS Config console at [https://console\.aws\.amazon\.com/config](https://console.aws.amazon.com/config)\. 

1. On the **Resource inventory** page, choose **Resources**\. Scroll down and select the **CodeBuild project** check box\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/config-select-project.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. Choose **Look up**\.

1. After the list of CodeBuild projects is added, choose the CodeBuild project name link in the **Config timeline** column\.

## Viewing AWS CodeBuild configuration details in the AWS Config console<a name="viewing-config-details"></a>

When you look up resources on the **Resource inventory** page, you can choose the AWS Config timeline to view details about your CodeBuild project\. The details page for a resource provides information about the configuration, relationships, and number of changes made to that resource\. 

The blocks at the top of the page are collectively called the timeline\. The timeline shows the date and time that the recording was made\.

For more information, see [Viewing configuration details in the AWS Config console](https://docs.aws.amazon.com/config/latest/developerguide/view-manage-resource-console.html) in the *AWS Config Developer Guide*\.

Example of a CodeBuild project in AWS Config:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/config-resources.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)