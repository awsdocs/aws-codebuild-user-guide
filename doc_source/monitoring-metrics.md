# Monitoring CodeBuild metrics<a name="monitoring-metrics"></a>

 AWS CodeBuild monitors functions on your behalf and reports metrics through Amazon CloudWatch\. These metrics include the number of total builds, failed builds, successful builds, and the duration of builds\. 

 You can use the CodeBuild console or the CloudWatch console to monitor metrics for CodeBuild\. The following procedures show you how to access metrics\. 

**Topics**
+ [Access build metrics \(CodeBuild console\)](#metrics-in-codebuild-console)
+ [Access build metrics \(Amazon CloudWatch console\)](#metrics-in-cloudwatch-console)

## Access build metrics \(CodeBuild console\)<a name="metrics-in-codebuild-console"></a>

**Note**  
You can't customize the metrics or the graphs used to display them in the CodeBuild console\. If you want to customize the display, use the Amazon CloudWatch console to view your build metrics\. 

### Account\-level metrics<a name="codebuild-console-account-level-metrics"></a><a name="cw-account-metrics-codebuild-console"></a>

**To access AWS account\-level metrics**

1. Sign in to the AWS Management Console and open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  In the navigation pane, choose **Account metrics**\. 

### Project\-level metrics<a name="codebuild-console-project-level-metrics"></a><a name="cw-project-codebuild-console"></a>

**To access project\-level metrics**

1. Sign in to the AWS Management Console and open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  In the navigation pane, choose **Build projects**\. 

1.  In the list of build projects, in the **Name** column, choose the project where you want to view metrics\. 

1.  Choose the **Metrics** tab\. 

## Access build metrics \(Amazon CloudWatch console\)<a name="metrics-in-cloudwatch-console"></a>

You can customize the metrics and the graphs used to display them with the CloudWatch console\. 

### Account\-level metrics<a name="cloudwatch-console-account-level-metrics"></a><a name="cw-account-cloudwatch-console"></a>

**To access account\-level metrics**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Metrics**\. 

1.  On the **All metrics** tab, choose **CodeBuild**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/codebuild-metrics-in-cw.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  Choose **Account Metrics**\. 

1.  Choose one or more projects and metrics\. For each project, you can choose the **SucceededBuilds**, **FailedBuilds**, **Builds**, and **Duration** metrics\. All selected project and metric combinations are displayed in the graph on the page\. 

### Project\-level metrics<a name="cloudwatch-console-project-level-metrics"></a><a name="cw-project-cloudwatch-console"></a>

**To access project\-level metrics**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Metrics**\. 

1.  On the **All metrics** tab, choose **CodeBuild**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/codebuild-metrics-in-cw.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  Choose **By Project**\. 

1.  Choose one or more project and metric combinations\. For each project, you can choose the **SucceededBuilds**, **FailedBuilds**, **Builds**, and **Duration** metrics\. All selected project and metric combinations are displayed in the graph on the page\. 

1.  \(Optional\) You can customize your metrics and graphs\. For example, from the drop\-down list in the **Statistic** column, you can choose a different statistic to display\. Or from the drop\-down menu in the **Period** column, you can choose a different time period to use to monitor the metrics\. 

   For more information, see [Graph metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/graph_metrics.html) and [View available metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html) in the *Amazon CloudWatch User Guide*\. 