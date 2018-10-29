--------

A new console design is available for this service\. Although the procedures in this guide were written for the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\.

--------

# View Build Details in AWS CodeBuild<a name="view-build-details"></a>

To view details about builds managed by AWS CodeBuild, you can use the AWS CodeBuild console, AWS CLI, or AWS SDKs\.

**Topics**
+ [View Build Details \(Console\)](#view-build-details-console)
+ [View Build Details \(AWS CLI\)](#view-build-details-cli)
+ [View Build Details \(AWS SDKs\)](#view-build-details-sdks)
+ [Build Phase Transitions](#view-build-details-phases)

## View Build Details \(Console\)<a name="view-build-details-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. Do one of the following:
   + In the navigation pane, choose **Build history**\. In the list of builds, in the **Build run** column, choose the link that corresponds to the build\. 
   + In the navigation pane, choose **Build projects**\. In the list of build projects, in the **Project** column, choose the link that corresponds to the name of the build project\. Then, in the list of builds, in the **Build run** column, choose the link that corresponds to the build\.
**Note**  
By default, only the ten most recent builds or build projects are displayed\. To view more builds or build projects, select a different value for **Builds per page** or **Projects per page** or select the back and forward arrows for **Viewing builds** or **Viewing projects**\.

## View Build Details \(AWS CLI\)<a name="view-build-details-cli"></a>

For more information about using the AWS CLI with AWS CodeBuild, see the [Command Line Reference](cmd-ref.md)\.

Run the `batch-get-builds` command:

```
aws codebuild batch-get-builds --ids ids
```

Replace the following placeholder:
+ *ids*: Required string\. One or more build IDs to view details about\. To specify more than one build ID, separate each build ID with a space\. You can specify up to 100 build IDs\. To get a list of build IDs, see one or more of the following topics:
  + [View a List of Build IDs \(AWS CLI\)](view-build-list.md#view-build-list-cli)
  + [View a List of Build IDs for a Build Project \(AWS CLI\)](view-builds-for-project.md#view-builds-for-project-cli)

For example, if you run this command:

```
aws codebuild batch-get-builds --ids codebuild-demo-project:e9c4f4df-3f43-41d2-ab3a-60fe2EXAMPLE codebuild-demo-project:815e755f-bade-4a7e-80f0-efe51EXAMPLE my-other-project:813bb6c6-891b-426a-9dd7-6d8a3EXAMPLE
```

If the command is successful, data similar to that described in the [To view summarized build information \(AWS CLI\)](getting-started.md#getting-started-monitor-build-cli) procedure in Getting Started will appear in the output\.

## View Build Details \(AWS SDKs\)<a name="view-build-details-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.

## Build Phase Transitions<a name="view-build-details-phases"></a>

Builds in AWS CodeBuild proceed in phases:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/build-phases.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

An important point to note here is that the `UPLOAD_ARTIFACTS` phase is always attempted, even if the `BUILD` phase fails\.