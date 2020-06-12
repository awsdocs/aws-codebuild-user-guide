# Step 8: View detailed build information<a name="getting-started-cli-build-log"></a>

\(Previous step: [Step 7: View summarized build information](getting-started-cli-monitor-build.md)\)

In this step, you view detailed information about your build in CloudWatch Logs\.

**Note**  
 To protect sensitive information, the following are hidden in CodeBuild logs:   
 AWS access key IDs\. For more information, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *AWS Identity and Access Management User Guide*\. 
 Strings specified using the Parameter Store\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\. 
 Strings specified using AWS Secrets Manager\. For more information, see [Key management](security-key-management.md)\. <a name="getting-started-cli-build-log-cli"></a>

**To view detailed build information**

1. Use your web browser to go to the `deepLink` location that appeared in the output in the previous step \(for example, `https://console.aws.amazon.com/cloudwatch/home?region=region-ID#logEvent:group=/aws/codebuild/codebuild-demo-project;stream=38ca1c4a-e9ca-4dbc-bef1-d52bfEXAMPLE`\)\.

1. In the CloudWatch Logs log stream, you can browse the log events\. By default, only the last set of log events is displayed\. To see earlier log events, scroll to the beginning of the list\.

1. In this tutorial, most of the log events contain verbose information about CodeBuild downloading and installing build dependency files into its build environment, which you probably don't care about\. You can use the **Filter events** box to reduce the information displayed\. For example, if you enter `"[INFO]"` in **Filter events**, only those events that contain `[INFO]` are displayed\. For more information, see [Filter and pattern syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html) in the *Amazon CloudWatch User Guide*\.

These portions of a CloudWatch Logs log stream pertain to this tutorial\.

```
...
[Container] 2016/04/15 17:49:42 Entering phase PRE_BUILD 
[Container] 2016/04/15 17:49:42 Running command echo Entering pre_build phase...
[Container] 2016/04/15 17:49:42 Entering pre_build phase... 
[Container] 2016/04/15 17:49:42 Phase complete: PRE_BUILD Success: true 
[Container] 2016/04/15 17:49:42 Entering phase BUILD 
[Container] 2016/04/15 17:49:42 Running command echo Entering build phase... 
[Container] 2016/04/15 17:49:42 Entering build phase...
[Container] 2016/04/15 17:49:42 Running command mvn install 
[Container] 2016/04/15 17:49:44 [INFO] Scanning for projects... 
[Container] 2016/04/15 17:49:44 [INFO]
[Container] 2016/04/15 17:49:44 [INFO] ------------------------------------------------------------------------ 
[Container] 2016/04/15 17:49:44 [INFO] Building Message Utility Java Sample App 1.0 
[Container] 2016/04/15 17:49:44 [INFO] ------------------------------------------------------------------------ 
... 
[Container] 2016/04/15 17:49:55 ------------------------------------------------------- 
[Container] 2016/04/15 17:49:55  T E S T S 
[Container] 2016/04/15 17:49:55 ------------------------------------------------------- 
[Container] 2016/04/15 17:49:55 Running TestMessageUtil 
[Container] 2016/04/15 17:49:55 Inside testSalutationMessage() 
[Container] 2016/04/15 17:49:55 Hi!Robert 
[Container] 2016/04/15 17:49:55 Inside testPrintMessage() 
[Container] 2016/04/15 17:49:55 Robert 
[Container] 2016/04/15 17:49:55 Tests run: 2, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.018 sec
[Container] 2016/04/15 17:49:55  
[Container] 2016/04/15 17:49:55 Results : 
[Container] 2016/04/15 17:49:55  
[Container] 2016/04/15 17:49:55 Tests run: 2, Failures: 0, Errors: 0, Skipped: 0 
...
[Container] 2016/04/15 17:49:56 [INFO] ------------------------------------------------------------------------ 
[Container] 2016/04/15 17:49:56 [INFO] BUILD SUCCESS 
[Container] 2016/04/15 17:49:56 [INFO] ------------------------------------------------------------------------ 
[Container] 2016/04/15 17:49:56 [INFO] Total time: 11.845 s 
[Container] 2016/04/15 17:49:56 [INFO] Finished at: 2016-04-15T17:49:56+00:00 
[Container] 2016/04/15 17:49:56 [INFO] Final Memory: 18M/216M 
[Container] 2016/04/15 17:49:56 [INFO] ------------------------------------------------------------------------ 
[Container] 2016/04/15 17:49:56 Phase complete: BUILD Success: true 
[Container] 2016/04/15 17:49:56 Entering phase POST_BUILD 
[Container] 2016/04/15 17:49:56 Running command echo Entering post_build phase... 
[Container] 2016/04/15 17:49:56 Entering post_build phase... 
[Container] 2016/04/15 17:49:56 Phase complete: POST_BUILD Success: true 
[Container] 2016/04/15 17:49:57 Preparing to copy artifacts 
[Container] 2016/04/15 17:49:57 Assembling file list 
[Container] 2016/04/15 17:49:57 Expanding target/messageUtil-1.0.jar 
[Container] 2016/04/15 17:49:57 Found target/messageUtil-1.0.jar 
[Container] 2016/04/15 17:49:57 Creating zip artifact
```

In this example, CodeBuild successfully completed the pre\-build, build, and post\-build build phases\. It ran the unit tests and successfully built the `messageUtil-1.0.jar` file\.

## Next step<a name="getting-started-cli-build-log-next"></a>

[Step 9: Get the build output artifact](getting-started-cli-output.md)