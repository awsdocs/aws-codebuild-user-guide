# Stop a batch build in AWS CodeBuild<a name="stop-batch-build"></a>

You can use the AWS CodeBuild console, AWS CLI,or AWS SDKs to stop a batch build in AWS CodeBuild\.

**Topics**
+ [Stop a batch build \(console\)](#stop-batch-build-console)
+ [Stop a batch build \(AWS CLI\)](#stop-batch-build-cli)
+ [Stop a batch build \(AWS SDKs\)](#stop-batch-build-sdks)

## Stop a batch build \(console\)<a name="stop-batch-build-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. Do one of the following:
   + If the ***build\-project\-name*:*build\-ID*** page is displayed, choose **Stop build**\.
   + In the navigation pane, choose **Build history**\. In the list of builds, select the box for the build, and then choose **Stop build**\.
   + In the navigation pane, choose **Build projects**\. In the list of build projects, in the **Name** column, choose the link for the build project's name\. In the list of builds, select the box for the build, and then choose **Stop build**\.

**Note**  
By default, only the most recent 100 builds or build projects are displayed\. To view more builds or build projects, choose the gear icon, and then choose a different value for **Builds per page** or **Projects per page** or use the back and forward arrows\.  
If AWS CodeBuild cannot successfully stop a batch build \(for example, if the build process is already complete\), the **Stop build** button is disabled\.

## Stop a batch build \(AWS CLI\)<a name="stop-batch-build-cli"></a>
+ Run the [https://docs.aws.amazon.com/cli/latest/reference/codebuild/stop-build-batch.html](https://docs.aws.amazon.com/cli/latest/reference/codebuild/stop-build-batch.html) command:

  ```
  aws codebuild stop-build-batch --id <batch-build-id>
  ```

  In the preceding command, replace the following placeholder:
  + *<batch\-build\-id>*: Required string\. The identifier of the batch build to stop\. To get a list of batch build identifiers, see the following topics:
    + [View a list of batch build IDs \(AWS CLI\)](view-build-list.md#view-batch-build-list-cli)
    + [View a list of batch build IDs for a build project \(AWS CLI\)](view-builds-for-project.md#view-batch-builds-for-project-cli)

  If AWS CodeBuild successfully stops the batch build, the `buildBatchStatus` value in the `buildBatch` object in the output is `STOPPED`\.

  If CodeBuild cannot successfully stop the batch build \(for example, if the batch build is already complete\), the `buildBatchStatus` value in the `buildBatch` object in the output is the final build status \(for example, `SUCCEEDED`\)\.

## Stop a batch build \(AWS SDKs\)<a name="stop-batch-build-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and tools reference](sdk-ref.md)\.