# Retry a build in AWS CodeBuild<a name="retry-build"></a>

You can use the AWS CodeBuild console, AWS CLI,or AWS SDKs to retry either a single build or a batch build in AWS CodeBuild\.

**Topics**
+ [Retry a build \(console\)](#retry-build-console)
+ [Retry a build \(AWS CLI\)](#retry-build-cli)
+ [Retry a build \(AWS SDKs\)](#retry-build-sdks)

## Retry a build \(console\)<a name="retry-build-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. Do one of the following:
   + If the ***build\-project\-name*:*build\-ID*** page is displayed, choose **Retry build**\.
   + In the navigation pane, choose **Build history**\. In the list of builds, select the box for the build, and then choose **Retry build**\.
   + In the navigation pane, choose **Build projects**\. In the list of build projects, in the **Name** column, choose the link for the build project's name\. In the list of builds, select the box for the build, and then choose **Retry build**\.

**Note**  
By default, only the most recent 100 builds or build projects are displayed\. To view more builds or build projects, choose the gear icon, and then choose a different value for **Builds per page** or **Projects per page** or use the back and forward arrows\.

## Retry a build \(AWS CLI\)<a name="retry-build-cli"></a>
+ Run the retry\-build command:

  ```
  aws codebuild retry-build --id <build-id> --idempotency-token <idempotencyToken>
  ```

  In the preceding command, replace the following placeholder:
  + *<build\-id>*: Required string\. The ID of the build or batch build to retry\. To get a list of build IDs, see the following topics:
    + [View a list of build IDs \(AWS CLI\)](view-build-list.md#view-build-list-cli)
    + [View a list of batch build IDs \(AWS CLI\)](view-build-list.md#view-batch-build-list-cli)
    + [View a list of build IDs for a build project \(AWS CLI\)](view-builds-for-project.md#view-builds-for-project-cli)
    + [View a list of batch build IDs for a build project \(AWS CLI\)](view-builds-for-project.md#view-batch-builds-for-project-cli)
  + `--idempotency-token`: Optional\. If you run the retry\-build command with the option, a unique case\-sensitive identifier, or token, is included with the `retry-build` request\. The token is valid for 5 minutes after the request\. If you repeat the `retry-build` request with the same token, but change a parameter, CodeBuild returns a parameter mismatch error\.

## Retry a build \(AWS SDKs\)<a name="retry-build-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and tools reference](sdk-ref.md)\.