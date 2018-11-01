--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Stop a Build in AWS CodeBuild<a name="stop-build"></a>

You can use the AWS CodeBuild console, AWS CLI,or AWS SDKs to stop a build in AWS CodeBuild\.

**Topics**
+ [Stop a Build \(Console\)](#stop-build-console)
+ [Stop a Build \(AWS CLI\)](#stop-build-cli)
+ [Stop a Build \(AWS SDKs\)](#stop-build-sdks)

## Stop a Build \(Console\)<a name="stop-build-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/](https://console.aws.amazon.com/codesuite/codebuild/)\.

1. Do one of the following:
   + If the ***build\-project\-name*:*build\-ID*** page is displayed, choose **Stop build**\.
   + In the navigation pane, choose **Build history**\. In the list of builds, select the box for the build, and then choose **Stop build**\.
   + In the navigation pane, choose **Build projects**\. In the list of build projects, in the **Name** column, choose the link for the build project's name\. In the list of builds, select the box for the build, and then choose **Stop build**\.

**Note**  
By default, only the most recent 100 builds or build projects are displayed\. To view more builds or build projects, choose the gear icon, and then choose a different value for **Builds per page** or **Projects per page** or use the back and forward arrows\.  
If AWS CodeBuild cannot successfully stop a build \(for example, if the build process is already complete\), the **Stop** button is disabled or might not appear\.

## Stop a Build \(AWS CLI\)<a name="stop-build-cli"></a>
+ Run the stop\-build command:

  ```
  aws codebuild stop-build --id id
  ```

  In the preceding command, replace the following placeholder:
  + *id*: Required string\. The ID of the build to stop\. To get a list of build IDs, see the following topics:
    + [View a List of Build IDs \(AWS CLI\)](view-build-list.md#view-build-list-cli)
    + [View a List of Build IDs for a Build Project \(AWS CLI\)](view-builds-for-project.md#view-builds-for-project-cli)

  If AWS CodeBuild successfully stops the build, the `buildStatus` value in the `build` object in the output is `STOPPED`\.

  If AWS CodeBuild cannot successfully stop the build \(for example, if the build is already complete\), the `buildStatus` value in the `build` object in the output is the final build status \(for example, `SUCCEEDED`\)\.

## Stop a Build \(AWS SDKs\)<a name="stop-build-sdks"></a>

For more information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.