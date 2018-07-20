# Monitoring AWS CodeBuild<a name="monitoring-builds"></a>

 You can use Amazon CloudWatch to watch your builds, report when something is wrong, and take automatic actions when appropriate\. You can monitor your builds at two levels: 
+  At the project level: These metrics are for all builds in the specified project only\. To see metrics for a project, specify the `ProjectName` for the dimension in CloudWatch\. 
+  At the AWS account level: These metrics are for all builds in one account\. To see metrics at the AWS account level, do not enter a dimension in CloudWatch\. 

 CloudWatch metrics show the behavior of your builds over time\. For example, you can monitor: 
+  How many builds were attempted in a build project or an AWS account over time\. 
+  How many builds were successful in a build project or an AWS account over time\. 
+  How many builds failed in a build project or an AWS account over time\. 
+  How much time AWS CodeBuild spent executing builds in a build project or an AWS account over time\. 

 Metrics displayed in the AWS CodeBuild console are always from the past three days\. You can use the CloudWatch console to view AWS CodeBuild metrics over different durations\. 

 For information about creating CloudWatch metrics for AWS CodeBuild, see [Monitoring Builds with CloudWatch Metrics](monitoring-metrics.md)\. 

## AWS CodeBuild CloudWatch Metrics<a name="cloudwatch_metrics-codebuild"></a>

 The following metrics can be tracked per AWS account, or per AWS build project\. 


****  

|   Metric   |   Description   | 
| --- | --- | 
|   Builds   |   Measures the number of builds triggered\.   Units: Count   Valid CloudWatch statistics: Sum   | 
|   Duration   |   Measures the duration of all builds over time\.   Units: Seconds   Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum   | 
|   SucceededBuilds   |   Measures the number of successful builds\.   Units: Count   Valid CloudWatch statistics: Sum   | 
|   FailedBuilds   |   Measures the number of builds that failed because of client error or because of a timeout\.   Units: Count   Valid CloudWatch statistics: Sum   | 

## AWS CodeBuild CloudWatch Dimensions<a name="codebuild-cloudwatch-dimensions"></a>

 `ProjectName ` is the only AWS CodeBuild metrics dimension\. If it is specified, then the metrics are for that project\. If it is not specified, then the metrics are for the current AWS account\. 


****  

|   Dimension   |   Description   | 
| --- | --- | 
|   ProjectName   |   If specified, then the metrics are for that project\. If not specified, then the metrics are for the AWS account\.   | 

## AWS CodeBuild CloudWatch Alarms<a name="codebuild_cloudwatch_alarms"></a>

 You can use the CloudWatch console to create alarms based on AWS CodeBuild metrics so you can react if something goes wrong with your builds\. The two metrics that are most useful with alarms are: 
+  FailedBuild\. In CloudWatch you can create an alarm that is triggered when a certain number of failed builds are detected within a predetermined number of seconds\. In CloudWatch you specify the number of seconds and how many faild builds will trigger an alarm\. 
+  Duration\. In CloudWatch you can create an alarm that is triggered when a build takes longer than expected\. You specify how many seconds must elapse after a build is started and before a build completed in order to trigger an alarm\. 

 For information about how to create alarms for AWS CodeBuild metrics, see [Monitoring Builds with CloudWatch Alarms](monitoring-alarms.md)\. For more information about alarms, see [ Creating Amazon CloudWatch Alarms ](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)\. 