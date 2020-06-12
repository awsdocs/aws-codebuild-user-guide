# Step 8: View detailed build information<a name="getting-started-build-log-console"></a>

\(Previous step: [Step 7: View summarized build information](getting-started-monitor-build-console.md)\)

In this step, you view detailed information about your build in CloudWatch Logs\.

**Note**  
 To protect sensitive information, the following are hidden in CodeBuild logs:   
 AWS access key IDs\. For more information, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *AWS Identity and Access Management User Guide*\. 
 Strings specified using the Parameter Store\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\. 
 Strings specified using AWS Secrets Manager\. For more information, see [Key management](security-key-management.md)\. <a name="getting-started-build-log-console-procedure"></a>

**To view detailed build information**

1. With the build details page still displayed from the previous step, the last 10,000 lines of the build log are displayed in **Build logs**\. To see the entire build log in CloudWatch Logs, choose the **View entire log** link\. 

1. In the CloudWatch Logs log stream, you can browse the log events\. By default, only the last set of log events is displayed\. To see earlier log events, scroll to the beginning of the list\.

1. In this tutorial, most of the log events contain verbose information about CodeBuild downloading and installing build dependency files into its build environment, which you probably don't care about\. You can use the **Filter events** box to reduce the information displayed\. For example, if you enter `"[INFO]"` in **Filter events**, only those events that contain `[INFO]` are displayed\. For more information, see [Filter and pattern syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/FilterAndPatternSyntax.html) in the *Amazon CloudWatch User Guide*\.

## Next step<a name="getting-started-build-log-console-next"></a>

[Step 9: Get the build output artifact](getting-started-output-console.md)