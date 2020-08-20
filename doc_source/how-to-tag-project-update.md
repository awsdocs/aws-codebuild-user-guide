# Edit tags for a project<a name="how-to-tag-project-update"></a>

You can change the value for a tag associated with a project\. You can also change the name of the key, which is equivalent to removing the current tag and adding a different one with the new name and the same value as the other key\. Keep in mind that there are limits on the characters you can use in the key and value fields\. For more information, see [Tags](limits.md#tag-limits)\.

**Important**  
Editing tags for a project can impact access to that project\. Before you edit the name \(key\) or value of a tag for a project, make sure to review any IAM policies that might use the key or value for a tag to control access to resources such as build projects\. For examples of tag\-based access policies, see [Using tags to control access to AWS CodeBuild resources](auth-and-access-control-using-tags.md)\.

## Edit a tag for a project \(console\)<a name="how-to-tag-project-update-console"></a>

You can use the CodeBuild console to edit the tags associated with a CodeBuild project\. 

1. Open the CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. In **Build projects**, choose the name of the project where you want to edit tags\.

1. In the navigation pane, choose **Settings**\. Choose **Build project tags**\. 

1. Choose **Edit**\.

1. Do one of the following:
   + To change the tag, enter a new name in **Key**\. Changing the name of the tag is the equivalent of removing a tag and adding a new tag with the new key name\.
   + To change the value of a tag, enter a new value\. If you want to change the value to nothing, delete the current value and leave the field blank\.

1. When you have finished editing tags, choose **Submit**\.

## Edit tags for a project \(AWS CLI\)<a name="how-to-tag-project-update-cli"></a>

 To add, change, or delete tags from a build project, see [Change a build project's settings \(AWS CLI\)](change-project-cli.md)\. Update the `tags` section in the JSON\-formatted data you use to update the project\. 