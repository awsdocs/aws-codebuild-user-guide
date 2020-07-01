# Monitoring CodeBuild resource utilization metrics<a name="monitoring-utilization-metrics"></a>

AWS CodeBuild monitors build resource utilization on your behalf and reports metrics through Amazon CloudWatch\. These include metrics such as CPU, memory, and storage utilization\.

You can use the CodeBuild console or the CloudWatch console to monitor resource utilization metrics for CodeBuild\. The following procedures show you how to access your resource utilization metrics\.

**Note**  
CodeBuild resource utilization metrics are only available in the following regions:  
Asia Pacific \(Tokyo\) Region
Asia Pacific \(Seoul\) Region
Asia Pacific \(Mumbai\) Region
Asia Pacific \(Singapore\) Region
Asia Pacific \(Sydney\) Region
Canada \(Central\) Region
Europe \(Frankfurt\) Region
Europe \(Ireland\) Region
Europe \(London\) Region
Europe \(Paris\) Region
South America \(São Paulo\) Region
US East \(N\. Virginia\) Region
US East \(Ohio\) Region
US West \(N\. California\) Region
US West \(Oregon\) Region

**Topics**
+ [Access resource utilization metrics \(CodeBuild console\)](#utilization-metrics-codebuild-console)
+ [Access resource utilization metrics \(Amazon CloudWatch console\)](#utilization-metrics-cloudwatch-console)

## Access resource utilization metrics \(CodeBuild console\)<a name="utilization-metrics-codebuild-console"></a>

**Note**  
You can't customize the metrics or the graphs used to display them in the CodeBuild console\. If you want to customize the display, use the Amazon CloudWatch console to view your build metrics\. 

### Project\-level resource utilization metrics<a name="codebuild-console-project-level-utilization"></a>

**To access project\-level resource utilization metrics**

1. Sign in to the AWS Management Console and open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the navigation pane, choose **Build projects**\.

1. In the list of build projects, in the **Name** column, choose the project you want to view the utilization metrics for\.

1. Choose the **Metrics** tab\. The resource utilization metrics are displayed in the **Resource utilization metrics** section\.

1. To view the project\-level resource utilization metrics in the CloudWatch console, choose **View in CloudWatch** in the **Resource utilization metrics** section\.

### Build\-level resource utilization metrics<a name="codebuild-console-build-level-utilization"></a>

**To access build\-level resource utilization metrics**

1. Sign in to the AWS Management Console and open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the navigation pane, choose **Build history**\.

1. In the list of builds, in the **Build run** column, choose the build you want to view the utilization metrics for\.

1. Choose the **Resource utilization** tab\.

1. To view the build\-level resource utilization metrics in the CloudWatch console, choose **View in CloudWatch** in the **Resource utilization metrics** section\.

## Access resource utilization metrics \(Amazon CloudWatch console\)<a name="utilization-metrics-cloudwatch-console"></a>

The Amazon CloudWatch console can be used to access CodeBuild resource utilization metrics\.

### Project\-level resource utilization metrics<a name="cloudwatch-console-project-level-utilization"></a><a name="cw-project-cloudwatch-console"></a>

**To access project\-level resource utilization metrics**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. On the **All metrics** tab, choose **CodeBuild**\.  
![\[Console screenshot showing the CodeBuild option located on the All metrics tab.\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/codebuild-metrics-in-cw.png)![\[Console screenshot showing the CodeBuild option located on the All metrics tab.\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Console screenshot showing the CodeBuild option located on the All metrics tab.\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. Choose **By Project**\.

1. Choose one or more project and metric combinations to add to the graph\. All selected project and metric combinations are displayed in the graph on the page\.

1. \(Optional\) You can customize your metrics and graphs from the **Graphed metrics** tab\. For example, from the drop\-down list in the **Statistic** column, you can choose a different statistic to display\. Or from the drop\-down menu in the **Period** column, you can choose a different time period to use to monitor the metrics\. 

   For more information, see [Graphing metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/graph_metrics.html) and [Viewing available metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html) in the *Amazon CloudWatch User Guide*\. 

### Build\-level resource utilization metrics<a name="cloudwatch-console-build-level-utilization"></a>

**To access build\-level resource utilization metrics**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. On the **All metrics** tab, choose **CodeBuild**\.  
![\[Console screenshot showing the CodeBuild option located on the All metrics tab.\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/codebuild-metrics-in-cw.png)![\[Console screenshot showing the CodeBuild option located on the All metrics tab.\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Console screenshot showing the CodeBuild option located on the All metrics tab.\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. Choose **BuildId, BuildNumber, ProjectName**\.

1. Choose one or more build and metric combinations to add to the graph\. All selected build and metric combinations are displayed in the graph on the page\.

1. \(Optional\) You can customize your metrics and graphs from the **Graphed metrics** tab\. For example, from the drop\-down list in the **Statistic** column, you can choose a different statistic to display\. Or from the drop\-down menu in the **Period** column, you can choose a different time period to use to monitor the metrics\. 

   For more information, see [Graphing metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/graph_metrics.html) and [Viewing available metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html) in the *Amazon CloudWatch User Guide*\. 