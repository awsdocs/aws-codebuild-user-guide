# Step 9: Get the build output artifact<a name="getting-started-output-console"></a>

\(Previous step: [Step 8: View detailed build information](getting-started-build-log-console.md)\)

In this step, you get the `messageUtil-1.0.jar` file that CodeBuild built and uploaded to the output bucket\.

You can use the CodeBuild console or the Amazon S3 console to complete this step\.

**To get the build output artifact \(AWS CodeBuild console\)**

1. With the CodeBuild console still open and the build details page still displayed from the previous step, choose the **Build details** tab and scroll down to the **Artifacts** section\. 
**Note**  
If the build details page is not displayed, in the navigation bar, choose **Build history**, and then choose the **Build run** link\.

1. The link to the Amazon S3 folder is under the **Artifacts upload location**\. This link opens the folder in Amazon S3 where you find the `messageUtil-1.0.jar` build output artifact file\.

**To get the build output artifact \(Amazon S3 console\)**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Open `codebuild-region-ID-account-ID-output-bucket`\.

1. Open the `codebuild-demo-project` folder\.

1. Open the `target` folder, where you find the `messageUtil-1.0.jar` build output artifact file\.

## Next step<a name="getting-started-output-console-next"></a>

[Step 10: Delete the S3 input bucket](getting-started-clean-up-console.md)