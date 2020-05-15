# Delete a build project in AWS CodeBuild<a name="delete-project"></a>

You can use the CodeBuild console, AWS CLI, or AWS SDKs to delete a build project in CodeBuild\. If you delete a project, its builds are not deleted\.

**Warning**  
You cannot delete a project that has builds and a resource policy\. To delete a project with a resource policy and builds, you must first remove the resource policy and delete its builds\. 

**Topics**
+ [Delete a build project \(console\)](#delete-project-console)
+ [Delete a build project \(AWS CLI\)](#delete-project-cli)
+ [Delete a build project \(AWS SDKs\)](#delete-project-sdks)

## Delete a build project \(console\)<a name="delete-project-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the navigation pane, choose **Build projects**\.

1. Do one of the following:
   + Choose the radio button next to the build project you want to delete, and then choose **Delete**\.
   + Choose the link for the build project you want to delete, and then choose **Delete**\.
**Note**  
By default, only the most recent 10 build projects are displayed\. To view more build projects, choose a different value for **Projects per page** or use the back and forward arrows for viewing projects\.

## Delete a build project \(AWS CLI\)<a name="delete-project-cli"></a>

1. Run the `delete-project` command:

   ```
   aws codebuild delete-project --name name
   ```

   Replace the following placeholder:
   + *name*: Required string\. The name of the build project to delete\. To get a list of available build projects, run the `list-projects` command\. For more information, see [View a list of build project names \(AWS CLI\)](view-project-list.md#view-project-list-cli)\.

1. If successful, no data and no errors appear in the output\.

For more information about using the AWS CLI with AWS CodeBuild, see the [Command line reference](cmd-ref.md)\.

## Delete a build project \(AWS SDKs\)<a name="delete-project-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and tools reference](sdk-ref.md)\.