# Run a build \(console\)<a name="run-build-console"></a>

To use AWS CodePipeline to run a build with CodeBuild, skip these steps and follow the instructions in [Use CodePipeline with CodeBuild](how-to-create-pipeline.md)\.

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the navigation pane, choose **Build projects**\.

1. In the list of build projects, choose the build project\.

1. You can run the build with the default build project settings, or override build settings for this build only\.

   1. If you want to run the build with the default build project settings, choose **Start build**\. The build starts immediately\.

   1. If you want to override the default build project settings, choose **Start build with overrides**\. In the **Start build** page, you can override the following:
      + **Build configuration**
      + **Source**
      + **Environment variable overrides**

      If you need to select more advanced overrides, choose **Advanced build overrides**\. In this page, you can override the following:
      + **Build configuration**
      + **Source**
      + **Environment**
      + **Buildspec**
      + **Artifacts**
      + **Logs**

      When you have made your override selections, choose **Start build**\.

For detailed information about this build, see [View build details \(console\)](view-build-details.md#view-build-details-console)\.