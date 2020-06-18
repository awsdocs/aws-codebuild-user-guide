# Monitoring AWS CodeBuild<a name="monitoring-builds"></a>

 You can use Amazon CloudWatch to watch your builds, report when something is wrong, and take automatic actions when appropriate\. You can monitor your builds at two levels: 
+  Project level: These metrics are for all builds in the specified project only\. To see metrics for a project, specify `ProjectName` for the dimension in CloudWatch\. 
+  AWS account level: These metrics are for all builds in one account\. To see metrics at the AWS account level, do not enter a dimension in CloudWatch\. 

 CloudWatch metrics show the behavior of your builds over time\. For example, you can monitor: 
+  How many builds were attempted in a build project or an AWS account over time\. 
+  How many builds were successful in a build project or an AWS account over time\. 
+  How many builds failed in a build project or an AWS account over time\. 
+  How much time CodeBuild spent executing builds in a build project or an AWS account over time\. 

 Metrics displayed in the CodeBuild console are always from the past three days\. You can use the CloudWatch console to view CodeBuild metrics over different durations\. 

 For more information, see [Monitoring builds with CloudWatch metrics](monitoring-metrics.md)\. 

## CodeBuild CloudWatch Metrics<a name="cloudwatch_metrics-codebuild"></a>

 The following metrics can be tracked per AWS account or build project\. 

BuildDuration  
Measures the duration of the build's `BUILD` phase\.  
Units: Seconds  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

Builds  
 Measures the number of builds triggered\.   
 Units: Count   
 Valid CloudWatch statistics: Sum 

DownloadSourceDuration  
Measures the duration of the build's `DOWNLOAD_SOURCE` phase\.  
Units: Seconds  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

Duration  
 Measures the duration of all builds over time\.   
 Units: Seconds   
 Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum 

FailedBuilds  
 Measures the number of builds that failed because of client error or a timeout\.   
 Units: Count   
 Valid CloudWatch statistics: Sum 

FinalizingDuration  
Measures the duration of the build's `FINALIZING` phase\.  
Units: Seconds  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

InstallDuration  
Measures the duration of the build's `INSTALL` phase\.  
Units: Seconds  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

PostBuildDuration  
Measures the duration of the build's `POST_BUILD` phase  
Units: Seconds  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

PreBuildDuration  
Measures the duration of the build's `PRE_BUILD` phase\.  
Units: Seconds  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

ProvisioningDuration  
Measures the duration of the build's `PROVISIONING` phase\.  
Units: Seconds  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

QueuedDuration  
Measures the duration of the build's `QUEUED` phase\.  
Units: Seconds  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

SubmittedDuration  
Measures the duration of the build's `SUBMITTED` phase\.  
Units: Seconds  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

SucceededBuilds  
 Measures the number of successful builds\.   
 Units: Count   
 Valid CloudWatch statistics: Sum 

UploadArtifactsDuration  
Measures the duration of the build's `UPLOAD_ARTIFACTS` phase\.  
Units: Seconds  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

## CodeBuild CloudWatch dimensions<a name="codebuild-cloudwatch-dimensions"></a>

 `ProjectName` is the only AWS CodeBuild metrics dimension\. If it is specified, then the metrics are for that project\. If it is not specified, then the metrics are for the current AWS account\. 

## CodeBuild CloudWatch alarms<a name="codebuild_cloudwatch_alarms"></a>

 You can use the CloudWatch console to create alarms based on CodeBuild metrics so you can react if something goes wrong with your builds\. The two metrics that are most useful with alarms are: 
+  `FailedBuild`\. You can create an alarm that is triggered when a certain number of failed builds are detected within a predetermined number of seconds\. In CloudWatch you specify the number of seconds and how many faild builds trigger an alarm\. 
+  `Duration`\. You can create an alarm that is triggered when a build takes longer than expected\. You specify how many seconds must elapse after a build is started and before a build is completed before the alarm is triggered\. 

 For information about how to create alarms for CodeBuild metrics, see [Monitoring builds with CloudWatch alarms](monitoring-alarms.md)\. For more information about alarms, see [Creating Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\. 