--------

A new console design is available for this service\. Although the procedures in this guide were written for the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\.

--------

# Stop a Build in AWS CodeBuild<a name="stop-build"></a>

To stop a build in AWS CodeBuild, you can use the AWS CodeBuild console, AWS CLI,or AWS SDKs\.

**Topics**
+ [Stop a Build \(Console\)](#stop-build-console)
+ [Stop a Build \(AWS CLI\)](#stop-build-cli)
+ [Stop a Build \(AWS SDKs\)](#stop-build-sdks)

## Stop a Build \(Console\)<a name="stop-build-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. Do one of the following:
   + If the ***build\-project\-name*:*build\-ID*** page is displayed, choose **Stop**\.
   + In the navigation pane, choose **Build history**\. In the list of builds, choose the box that corresponds to the build, and then choose **Stop**\.
   + In the navigation pane, choose **Build projects**\. In the list of build projects, in the **Project** column, choose the link that corresponds to the build project's name\. In the list of builds, choose the box that corresponds to the build, and then choose **Stop**\.

**Note**  
By default, only the most recent 10 builds or build projects are displayed\. To view more builds or build projects, select a different value for **Builds per page** or **Projects per page** or select the back and forward arrows for **Viewing builds** or **Viewing projects**\.  
If AWS CodeBuild cannot successfully stop a build \(for example, the build process is already complete\), the **Stop** button will be disabled or may be missing altogether\.

## Stop a Build \(AWS CLI\)<a name="stop-build-cli"></a>
+ Run the `stop-build` command:

  ```
  aws codebuild stop-build --id id
  ```

  In the preceding command, replace the following placeholder:
  + *id*: Required string\. The ID of the build to stop\. To get a list of build IDs, see the following topics:
    + [View a List of Build IDs \(AWS CLI\)](view-build-list.md#view-build-list-cli)
    + [View a List of Build IDs for a Build Project \(AWS CLI\)](view-builds-for-project.md#view-builds-for-project-cli)

  If AWS CodeBuild successfully stops the build, the `buildStatus` value in the `build` object in the output will be `STOPPED`\.

  If AWS CodeBuild cannot successfully stop the build \(for example, the build is already complete\), the `buildStatus` value in the `build` object in the output will be the final build status \(for example, `SUCCEEDED`\)\.

## Stop a Build \(AWS SDKs\)<a name="stop-build-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.