# View build details in AWS CodeBuild<a name="view-build-details"></a>

You can use the AWS CodeBuild console, AWS CLI, or AWS SDKs to view details about builds managed by CodeBuild\.

**Topics**
+ [View build details \(console\)](#view-build-details-console)
+ [View build details \(AWS CLI\)](#view-build-details-cli)
+ [View build details \(AWS SDKs\)](#view-build-details-sdks)
+ [Build phase transitions](#view-build-details-phases)

## View build details \(console\)<a name="view-build-details-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. Do one of the following:
   + In the navigation pane, choose **Build history**\. In the list of builds, in the **Build run** column, choose the link for the build\. 
   + In the navigation pane, choose **Build projects**\. In the list of build projects, in the **Name** column, choose the link for the name of the build project\. Then, in the list of builds, in the **Build run** column, choose the link for the build\.
**Note**  
By default, only the 10 most recent builds or build projects are displayed\. To view more builds or build projects, choose the gear icon, and then choose a different value for **Builds per page** or **Projects per page** or use the back and forward arrows\.

## View build details \(AWS CLI\)<a name="view-build-details-cli"></a>

For more information about using the AWS CLI with AWS CodeBuild, see the [Command line reference](cmd-ref.md)\.

Run the batch\-get\-builds command:

```
aws codebuild batch-get-builds --ids ids
```

Replace the following placeholder:
+ *ids*: Required string\. One or more build IDs to view details about\. To specify more than one build ID, separate each build ID with a space\. You can specify up to 100 build IDs\. To get a list of build IDs, see the following topics:
  + [View a list of build IDs \(AWS CLI\)](view-build-list.md#view-build-list-cli)
  + [View a list of build IDs for a build project \(AWS CLI\)](view-builds-for-project.md#view-builds-for-project-cli)

For example, if you run this command:

```
aws codebuild batch-get-builds --ids codebuild-demo-project:e9c4f4df-3f43-41d2-ab3a-60fe2EXAMPLE codebuild-demo-project:815e755f-bade-4a7e-80f0-efe51EXAMPLE my-other-project:813bb6c6-891b-426a-9dd7-6d8a3EXAMPLE
```

If the command is successful, data similar to that described in [To view summarized build information ](getting-started-cli-monitor-build.md#getting-started-cli-monitor-build-cli) appears in the output\.

## View build details \(AWS SDKs\)<a name="view-build-details-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and tools reference](sdk-ref.md)\.

## Build phase transitions<a name="view-build-details-phases"></a>

Builds in AWS CodeBuild proceed in phases:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/build-phases.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

**Important**  
The `UPLOAD_ARTIFACTS` phase is always attempted, even if the `BUILD` phase fails\.