# Add a tag to a project<a name="how-to-tag-project-add"></a>

Adding tags to a project can help you identify and organize your AWS resources and manage access to them\. First, you add one or more tags \(key\-value pairs\) to a project\. Keep in mind that there are limits on the number of tags you can have on a project\. There are restrictions on the characters you can use in the key and value fields\. For more information, see [Tags](limits.md#tag-limits)\. After you have tags, you can create IAM policies to manage access to the project based on these tags\. You can use the CodeBuild console or the AWS CLI to add tags to a project\. 

**Important**  
Before you add a tag to a project, make sure to review any IAM policies that might use tags to control access to resources such as build projects\. For examples of tag\-based access policies, see [Using tags to control access to AWS CodeBuild resources](auth-and-access-control-using-tags.md)\.

For more information about adding tags to a project when you create it, see [Add a tag to a project \(console\)](#how-to-tag-project-add-console)\.

**Topics**
+ [Add a tag to a project \(console\)](#how-to-tag-project-add-console)
+ [Add a tag to a project \(AWS CLI\)](#how-to-tag-project-add-cli)

## Add a tag to a project \(console\)<a name="how-to-tag-project-add-console"></a>

You can use the CodeBuild console to add one or more tags to a CodeBuild project\. 

1. Open the CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. In **Build projects**, choose the name of the project where you want to add tags\.

1. In the navigation pane, choose **Settings**\. Choose **Build project tags**\.

1. If no tags have been added to the project, choose **Add tag**\. Otherwise, choose **Edit**, and then choose **Add tag**\.

1. In **Key**, enter a name for the tag\. You can add an optional value for the tag in **Value**\. 

1. \(Optional\) To add another tag, choose **Add tag** again\.

1. When you have finished adding tags, choose **Submit**\.

## Add a tag to a project \(AWS CLI\)<a name="how-to-tag-project-add-cli"></a>

To add a tag to a project when you create it, see [Create a build project \(AWS CLI\)](create-project-cli.md)\. In `create-project.json`, add your tags\.

In these steps, we assume that you have already installed a recent version of the AWS CLI or updated to the current version\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

If successful, this command returns nothing\.