# Stop a build in AWS CodeBuild<a name="stop-build"></a>

You can use the AWS CodeBuild console, AWS CLI,or AWS SDKs to stop a build in AWS CodeBuild\.

**Topics**
+ [Stop a build \(console\)](#stop-build-console)
+ [Stop a build \(AWS CLI\)](#stop-build-cli)
+ [Stop a build \(AWS SDKs\)](#stop-build-sdks)

## Stop a build \(console\)<a name="stop-build-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. Do one of the following:
   + If the ***build\-project\-name*:*build\-ID*** page is displayed, choose **Stop build**\.
   + In the navigation pane, choose **Build history**\. In the list of builds, select the box for the build, and then choose **Stop build**\.
   + In the navigation pane, choose **Build projects**\. In the list of build projects, in the **Name** column, choose the link for the build project's name\. In the list of builds, select the box for the build, and then choose **Stop build**\.

**Note**  
By default, only the most recent 100 builds or build projects are displayed\. To view more builds or build projects, choose the gear icon, and then choose a different value for **Builds per page** or **Projects per page** or use the back and forward arrows\.  
If AWS CodeBuild cannot successfully stop a build \(for example, if the build process is already complete\), the **Stop** button is disabled or might not appear\.

## Stop a build \(AWS CLI\)<a name="stop-build-cli"></a>
+ Run the stop\-build command:

  ```
  aws codebuild stop-build --id id
  ```

  In the preceding command, replace the following placeholder:
  + *id*: Required string\. The ID of the build to stop\. To get a list of build IDs, see the following topics:
    + [View a list of build IDs \(AWS CLI\)](view-build-list.md#view-build-list-cli)
    + [View a list of build IDs for a build project \(AWS CLI\)](view-builds-for-project.md#view-builds-for-project-cli)

  If AWS CodeBuild successfully stops the build, the `buildStatus` value in the `build` object in the output is `STOPPED`\.

  If CodeBuild cannot successfully stop the build \(for example, if the build is already complete\), the `buildStatus` value in the `build` object in the output is the final build status \(for example, `SUCCEEDED`\)\.

## Stop a build \(AWS SDKs\)<a name="stop-build-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and tools reference](sdk-ref.md)\.