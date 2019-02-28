# Monitoring Builds with CloudWatch Metrics<a name="monitoring-metrics"></a>

 CodeBuild monitors functions on your behalf and reports metrics through Amazon CloudWatch\. These metrics include the number of total builds, failed builds, successful builds, and the duration of builds\. 

 You can use the CodeBuild console or the CloudWatch console to monitor metrics for CodeBuild\. The following procedures show you how to access metrics\. 

## Access Build Metrics \(CodeBuild Console\)<a name="metrics-in-codebuild-console"></a>

The graphs in the CodeBuild console show three days of metrics\. You cannot customize the metrics or the graphs used to display them\. Use the Amazon CloudWatch console to view your build metrics if you want to edit them\. <a name="cw-account-metrics-codebuild-console"></a>

**To access AWS account level metrics**

1. Sign in to the AWS Management Console and open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  In the navigation pane, choose **Account metrics**\. <a name="cw-project-codebuild-console"></a>

**To access project\-level metrics**

1. Sign in to the AWS Management Console and open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  In the navigation pane, choose **Build projects**\. 

1.  In the list of build projects, in the **Name** column, choose the project where you want to view metrics\. 

1.  Choose the **Metrics** tab\. 

## Access Build Metrics \(Amazon CloudWatch Console\)<a name="metrics-in-cloudwatch-console"></a>

 You can customize the metrics and the graphs used to display them\. <a name="cw-account-cloudwatch-console"></a>

**To access account level metrics**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Metrics**\. 

1.  On the **All metrics** tab, choose **CodeBuild**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/codebuild-metrics-in-cw.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  Choose **Account Metrics**\. 

1.  Choose one or more projects and metrics\. For each project, you can choose the **SucceededBuilds**, **FailedBuilds**, **Builds**, and **Duration** metrics\. All selected project and metric combinations are displayed in the graph on the page\. <a name="cw-project-cloudwatch-console"></a>

**To access project\-level metrics**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Metrics**\. 

1.  On the **All metrics** tab, choose **CodeBuild**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/codebuild-metrics-in-cw.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1.  Choose **By Project**\. 

1.  Choose one or more project and metric combinations\. For each project, you can choose the **SucceededBuilds**, **FailedBuilds**, **Builds**, and **Duration** metrics\. All selected project and metric combinations are displayed in the graph on the page\. 

1.  \(Optional\) You can customize your metrics and graphs\. For example, from the drop\-down list in the **Statistic** columm, you can choose a different statistic to display\. Or from the drop\-down menu in the **Period** column, you can choose a different time period to use to monitor the metrics\. For more information, see [Graph Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/graph_metrics.html) and [View Available Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html) in the *Amazon CloudWatch User Guide*\. 