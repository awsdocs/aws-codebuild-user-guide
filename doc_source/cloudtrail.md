# Logging AWS CodeBuild API Calls with AWS CloudTrail<a name="cloudtrail"></a>

AWS CodeBuild is integrated with CloudTrail, a service that captures API calls made by or on behalf of AWS CodeBuild in your AWS account and delivers the log files to an Amazon S3 bucket you specify\. CloudTrail captures API calls from the AWS CodeBuild console, the AWS CLI, and the AWS SDKs\. Using the information collected by CloudTrail, you can determine which request was made to AWS CodeBuild, the source IP address from which the request was made, who made the request, when it was made, and so on\. To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## AWS CodeBuild Information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

When CloudTrail logging is enabled in your AWS account, calls made to AWS CodeBuild actions are tracked in log files\. AWS CodeBuild records are written together with other AWS service records in a log file\. CloudTrail determines when to create and write to a new file based on a time period and file size\.

All of the AWS CodeBuild actions are logged and documented in the [Command Line Reference](cmd-ref.md)\. For example, calls to create build projects and run builds generate entries in CloudTrail log files\. 

Every log entry contains information about who generated the request\. The user identity information in the log helps you determine whether the request was made with root or IAM user credentials, with temporary security credentials for a role or federated user, or by another AWS service\. For more information, see the `userIdentity` field in the [CloudTrail Event Reference](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/event_reference_top_level.html)\.

You can store your log files in your bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, Amazon S3 server\-side encryption \(SSE\) is used to encrypt your log files\.

You can have CloudTrail publish Amazon SNS notifications when new log files are delivered\. For more information, see [Configuring Amazon SNS Notifications for CloudTrail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

You can also aggregate AWS CodeBuild log files from multiple AWS regions and multiple AWS accounts into a single Amazon S3 bucket\. For more information, see [Receiving CloudTrail Log Files from Multiple Regions](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/aggregating_logs_top_level.html)\.

## Understanding AWS CodeBuild Log File Entries<a name="understanding-service-name-entries"></a>

CloudTrail log files can contain one or more log entries where each entry is made up of multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, any parameters, the date and time of the action, and so on\. The log entries are not guaranteed to be in any particular order\. That is, they are not an ordered stack trace of the public calls\.

The following example shows a CloudTrail log entry that demonstrates creating a build project in AWS CodeBuild:

```
{    
  "eventVersion": "1.05",   
  "userIdentity": {       
    "type": "FederatedUser",       
    "principalId": "account-ID:user-name",       
    "arn": "arn:aws:sts::account-ID:federated-user/user-name",       
    "accountId": "account-ID",       
    "accessKeyId": "access-key-ID",       
    "sessionContext": {
      "attributes": {
        "mfaAuthenticated": "false",
        "creationDate": "2016-09-06T17:59:10Z"
      },
      "sessionIssuer": {
        "type": "IAMUser",
        "principalId": "access-key-ID",
        "arn": "arn:aws:iam::account-ID:user/user-name",
        "accountId": "account-ID",
        "userName": "user-name"
      }       
    }   
  },   
  "eventTime": "2016-09-06T17:59:11Z",   
  "eventSource": "codebuild.amazonaws.com",   
  "eventName": "CreateProject",   
  "awsRegion": "region-ID",   
  "sourceIPAddress": "127.0.0.1",   
  "userAgent": "user-agent",   
  "requestParameters": {       
    "awsActId": "account-ID"   
  },   
  "responseElements": {       
    "project": {
      "environment": {
        "image": "image-ID",
        "computeType": "BUILD_GENERAL1_SMALL",
        "type": "LINUX_CONTAINER",
        "environmentVariables": []
      },
      "name": "codebuild-demo-project",
      "description": "This is my demo project",
      "arn": "arn:aws:codebuild:region-ID:account-ID:project/codebuild-demo-project:project-ID",
      "encryptionKey": "arn:aws:kms:region-ID:key-ID",
      "timeoutInMinutes": 10,
      "artifacts": {
        "location": "arn:aws:s3:::codebuild-region-ID-account-ID-output-bucket",
        "type": "S3",
        "packaging": "ZIP",
        "outputName": "MyOutputArtifact.zip"
      }, 
      "serviceRole": "arn:aws:iam::account-ID:role/CodeBuildServiceRole",
      "lastModified": "Sep 6, 2016 10:59:11 AM",
      "source": {      
        "type": "GITHUB",
        "location": "https://github.com/my-repo.git"
      },
      "created": "Sep 6, 2016 10:59:11 AM"       
    }   
  },   
  "requestID": "9d32b228-745b-11e6-98bb-23b67EXAMPLE",   
  "eventID": "581f7dd1-8d2e-40b0-aeee-0dbf7EXAMPLE",   
  "eventType": "AwsApiCall",   
  "recipientAccountId": "account-ID" 
}
```