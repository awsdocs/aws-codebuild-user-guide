# Use AWS Config with AWS CodeBuild Sample<a name="how-to-integrate-config"></a>

AWS Config provides an inventory of your AWS resources and a history of configuration changes to these resources\. AWS Config now supports AWS CodeBuild as an AWS resource, which means the service can track your AWS CodeBuild projects\. For more information about AWS Config, see [What Is AWS Config?](http://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html) in the *AWS Config Developer Guide*\.

You can see the following information about AWS CodeBuild resources on the **Resource Inventory** page in the AWS Config console:

+ A timeline of your AWS CodeBuild configuration changes\.

+ Configuration details for each AWS CodeBuild project\.

+ Relationships with other AWS resources\.

+ A list of changes to your AWS CodeBuild projects\.

The procedures in this topic show you how to set up AWS Config and look up and view AWS CodeBuild projects\.


+ [Prerequisites](#how-to-create-a-build-project)
+ [Set Up AWS Config](#setup-config)
+ [Look Up AWS CodeBuild Projects](#lookup-projects)
+ [Viewing AWS CodeBuild Configuration Details in the AWS Config Console](#viewing-config-details)

## Prerequisites<a name="how-to-create-a-build-project"></a>

Create your AWS CodeBuild project\(s\)\. For more information, see [Create a Build Project](create-project.md)\.

## Set Up AWS Config<a name="setup-config"></a>

+ [Setting up AWS Config \(Console\)](http://docs.aws.amazon.com/config/latest/developerguide/gs-console.html)

+ [Setting up AWS Config \(AWS CLI\)](http://docs.aws.amazon.com/config/latest/developerguide/gs-cli.html)

**Note**  
It can take up to 10 minutes before a user is able to see AWS CodeBuild projects in the AWS Config console\.

## Look Up AWS CodeBuild Projects<a name="lookup-projects"></a>

1. Sign in to the AWS Management Console and open the AWS Config console at [https://console\.aws\.amazon\.com/config](https://console.aws.amazon.com/config)\. 

1. On the **Resource inventory** page, choose **Resources**\. Scroll down and select the **CodeBuild project** check box\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/config-select-project.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. Choose **Look up**\.

1. After the list of AWS CodeBuild projects is added, choose the AWS CodeBuild project name link in the **Config timeline** column\.

## Viewing AWS CodeBuild Configuration Details in the AWS Config Console<a name="viewing-config-details"></a>

When you look up resources on the **Resource inventory** page, you can choose the AWS Config timeline to view details about your AWS CodeBuild project\. The details page for a resource provides information about the configuration, relationships, and number of changes made to that resource\. 

The blocks at the top of the page are collectively called the timeline\. The timeline shows the date and time that the recording was made\.

For more information, see [Viewing Configuration Details in the AWS Config Console](http://docs.aws.amazon.com/config/latest/developerguide/view-manage-resource-console.html) in the *AWS Config Developer Guide*\.

**Example of a AWS CodeBuild Project in AWS Config:**

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/config-resources.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)