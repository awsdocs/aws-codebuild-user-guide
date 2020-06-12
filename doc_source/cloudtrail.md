# Logging AWS CodeBuild API calls with AWS CloudTrail<a name="cloudtrail"></a>

AWS CodeBuild is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in CodeBuild\. CloudTrail captures all API calls for CodeBuild as events, including calls from the CodeBuild console and from code calls to the CodeBuild APIs\. If you create a trail, you can enable continuous delivery of CloudTrail events to an S3 bucket, including events for CodeBuild\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. Using the information collected by CloudTrail, you can determine the request that was made to CodeBuild, the IP address from which the request was made, who made the request, when it was made, and additional details\. 

To learn more about CloudTrail, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## AWS CodeBuild information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When activity occurs in CodeBuild, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing events with CloudTrail event history](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html) in the *AWS CloudTrail User Guide*\. 

For an ongoing record of events in your AWS account, including events for CodeBuild, create a trail\. A trail enables CloudTrail to deliver log files to an S3 bucket\. By default, when you create a trail in the console, the trail applies to all regions\. The trail logs events from all regions in the AWS partition and delivers the log files to the S3 bucket that you specify\. You can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see: 
+ [Overview for creating a trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail supported services and integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations)
+ [Configuring Amazon SNS notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail log files from multiple regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail log files from multiple accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

All CodeBuild actions are logged by CloudTrail and are documented in the [CodeBuild API Reference](https://docs.aws.amazon.com/codebuild/latest/APIReference/)\. For example, calls to the `CreateProject` \(in the AWS CLI, `create-project`\), `StartBuild` \(in the AWS CLI, `start-project`\), and `UpdateProject` \(in the AWS CLI, `update-project`\) actions generate entries in the CloudTrail log files\. 

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 
+ Whether the request was made with root or IAM user credentials\.
+ Whether the request was made with temporary security credentials for a role or federated user\.
+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)in the *AWS CloudTrail User Guide*\.

## Understanding AWS CodeBuild log file entries<a name="understanding-service-name-entries"></a>

A trail is a configuration that enables delivery of events as log files to an S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files are not an ordered stack trace of the public API calls, so they do not appear in any specific order\. 

**Note**  
 To protect sensitive information, the following are hidden in CodeBuild logs:   
 AWS access key IDs\. For more information, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *AWS Identity and Access Management User Guide*\. 
 Strings specified using the Parameter Store\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\. 
 Strings specified using AWS Secrets Manager\. For more information, see [Key management](security-key-management.md)\. 

The following example shows a CloudTrail log entry that demonstrates creating a build project in CodeBuild\.

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