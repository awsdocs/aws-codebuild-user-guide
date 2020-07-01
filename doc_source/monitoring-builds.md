# Monitoring AWS CodeBuild<a name="monitoring-builds"></a>

You can use Amazon CloudWatch to watch your builds, report when something is wrong, and take automatic actions when appropriate\. You can monitor your builds at two levels: 

Project level  
These metrics are for all builds in the specified project\. To see metrics for a project, specify `ProjectName` for the dimension in CloudWatch\.

AWS account level  
These metrics are for all builds in an account\. To see metrics at the AWS account level, do not enter a dimension in CloudWatch\. Build resource utilization metrics are not available at the AWS account level\.

CloudWatch metrics show the behavior of your builds over time\. For example, you can monitor: 
+  How many builds were attempted in a build project or an AWS account over time\. 
+  How many builds were successful in a build project or an AWS account over time\. 
+  How many builds failed in a build project or an AWS account over time\. 
+  How much time CodeBuild spent executing builds in a build project or an AWS account over time\. 
+ Build resource utilization for a build or an entire build project\. Build resource utilization metrics include metrics such as CPU, memory, and storage utilization\.

 For more information, see [Monitoring CodeBuild metrics](monitoring-metrics.md)\. 

## CodeBuild CloudWatch metrics<a name="cloudwatch_metrics-codebuild"></a>

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

## CodeBuild CloudWatch resource utilization metrics<a name="cloudwatch-utilization-metrics"></a>

The following resource utilization metrics can be tracked\.

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

CPUUtilized  
The number of CPU units of allocated processing used by the build container\.  
Units: CPU units  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

CPUUtilizedPercent  
The percentage of allocated processing used by the build container\.  
Units: Percent  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

MemoryUtilized  
The number of megabytes of memory used by the build container\.  
Units: Megabytes  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

MemoryUtilizedPercent  
The percentage of allocated memory used by the build container\.  
Units: Percent  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

StorageReadBytes  
The storage read speed used by the build container\.  
Units: Bytes/second  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

StorageWriteBytes  
The storage write speed used by the build container\.  
Units: Bytes/second  
Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum

## CodeBuild CloudWatch dimensions<a name="codebuild-cloudwatch-dimensions"></a>

CodeBuild provides the following CloudWatch metric dimensions\. If none of these are specified, the metrics are for the current AWS account\. 

BuildId, BuildNumber, ProjectName  
Metrics are provided for a build identifier, build number, and project name\.

ProjectName  
Metrics are provided for a project name\.

## CodeBuild CloudWatch alarms<a name="codebuild_cloudwatch_alarms"></a>

 You can use the CloudWatch console to create alarms based on CodeBuild metrics so you can react if something goes wrong with your builds\. The two metrics that are most useful with alarms are: 
+  `FailedBuild`\. You can create an alarm that is triggered when a certain number of failed builds are detected within a predetermined number of seconds\. In CloudWatch, you specify the number of seconds and how many failed builds trigger an alarm\. 
+  `Duration`\. You can create an alarm that is triggered when a build takes longer than expected\. You specify how many seconds must elapse after a build is started and before a build is completed before the alarm is triggered\. 

 For information about how to create alarms for CodeBuild metrics, see [Monitoring builds with CloudWatch alarms](monitoring-alarms.md)\. For more information about alarms, see [Creating Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\. 