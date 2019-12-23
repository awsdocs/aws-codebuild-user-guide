# Step 9: Get the Build Output Artifact<a name="getting-started-cli-output"></a>

\(Previous step: [Step 8: View Detailed Build Information](getting-started-cli-build-log.md)\)

In this step, you get the `messageUtil-1.0.jar` file that CodeBuild built and uploaded to the output bucket\.

You can use the CodeBuild console or the Amazon S3 console to complete this step\.

**To get the build output artifact \(AWS CodeBuild console\)**

1. With the CodeBuild console still open and the build details page still displayed from the previous step, in **Build Status**, choose the **View artifacts** link\. This opens the folder in Amazon S3 for the build output artifact\. \(If the build details page is not displayed, in the navigation bar, choose **Build history**, and then choose the **Build run** link\.\)

1. Open the `target` folder, where you find the `messageUtil-1.0.jar` build output artifact file\.

:

**To get the build output artifact \(Amazon S3 console\)**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Open `codebuild-region-ID-account-ID-output-bucket`\.

1. Open the `codebuild-demo-project` folder\.

1. Open the `target` folder, where you find the `messageUtil-1.0.jar` build output artifact file\.

## Next Step<a name="getting-started-cli-output-next"></a>

[Step 10: Clean Up](getting-started-cli-clean-up.md)