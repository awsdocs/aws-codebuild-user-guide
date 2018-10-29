--------

A new console design is available for this service\. Although the procedures in this guide were written for the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\.

--------

# Delete a Build Project in AWS CodeBuild<a name="delete-project"></a>

You can use the AWS CodeBuild console, AWS CLI, or AWS SDKs to delete a build project in AWS CodeBuild\.

**Warning**  
If you delete a build project, it cannot be recovered\. All information about builds will also be deleted and cannot be recovered\.

**Topics**
+ [Delete a Build Project \(Console\)](#delete-project-console)
+ [Delete a Build Project \(AWS CLI\)](#delete-project-cli)
+ [Delete a Build Project \(AWS SDKs\)](#delete-project-sdks)

## Delete a Build Project \(Console\)<a name="delete-project-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. In the navigation pane, choose **Build projects**\.

1. Do one of the following:
   + Choose the radio button next to the build project you want to delete, choose **Actions**, and then choose **Delete**\.
   + Choose the link for the build project you want to delete, and then choose **Delete**\.
**Note**  
Only the most recent 10 build projects are displayed by default\. To view more build projects, select a different value for **Projects per page** or select the back and forward arrows for **Viewing projects**\.

## Delete a Build Project \(AWS CLI\)<a name="delete-project-cli"></a>

For more information about using the AWS CLI with AWS CodeBuild, see the [Command Line Reference](cmd-ref.md)\.

1. Run the `delete-project` command:

   ```
   aws codebuild delete-project --name name
   ```

   Replace the following placeholder:
   + *name*: Required string\. The name of the build project to delete\. To get a list of available build projects, run the `list-projects` command\. For more information, see [View a List of Build Project Names \(AWS CLI\)](view-project-list.md#view-project-list-cli)\.

1. If successful, no data and no errors appear in the output\.

## Delete a Build Project \(AWS SDKs\)<a name="delete-project-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.