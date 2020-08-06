# Build notifications sample for CodeBuild<a name="sample-build-notifications"></a>

Amazon CloudWatch Events has built\-in support for AWS CodeBuild\. CloudWatch Events is a stream of system events describing changes in your AWS resources\. With CloudWatch Events, you write declarative rules to associate events of interest with automated actions to be taken\. This sample uses Amazon CloudWatch Events and Amazon Simple Notification Service \(Amazon SNS\) to send build notifications to subscribers whenever builds succeed, fail, go from one build phase to another, or any combination of these events\.

**Important**  
Running this sample might result in charges to your AWS account\. These include possible charges for CodeBuild and for AWS resources and actions related to Amazon CloudWatch and Amazon SNS\. For more information, see [CodeBuild pricing](http://aws.amazon.com/codebuild/pricing), [Amazon CloudWatch pricing](http://aws.amazon.com/cloudwatch/pricing), and [Amazon SNS pricing](http://aws.amazon.com/sns/pricing)\.

## Running the sample<a name="sample-build-notifications-running"></a>

**To run this sample**

1. If you already have a topic set up and subscribed to in Amazon SNS that you want to use for this sample, skip ahead to step 4\. Otherwise, if you are using an IAM user instead of an AWS root account or an administrator IAM user to work with Amazon SNS, add the following statement \(between *\#\#\# BEGIN ADDING STATEMENT HERE \#\#\#* and *\#\#\# END ADDING STATEMENT HERE \#\#\#*\) to the user \(or IAM group the user is associated with\)\. Using an AWS root account is not recommended\. This statement enables viewing, creating, subscribing, and testing the sending of notifications to topics in Amazon SNS\. Ellipses \(`...`\) are used for brevity and to help you locate where to add the statement\. Do not remove any statements, and do not type these ellipses into the existing policy\.

   ```
   {
     "Statement": [
       ### BEGIN ADDING STATEMENT HERE ###
       {
         "Action": [
           "sns:CreateTopic",
           "sns:GetTopicAttributes",
           "sns:List*",
           "sns:Publish",
           "sns:SetTopicAttributes",
           "sns:Subscribe"
         ],
         "Resource": "*",
         "Effect": "Allow"
       },
       ### END ADDING STATEMENT HERE ###
       ...
     ],
     "Version": "2012-10-17"
   }
   ```
**Note**  
The IAM entity that modifies this policy must have permission in IAM to modify policies\.  
For more information, see [Editing customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html#edit-managed-policy-console) or the "To edit or delete an inline policy for a group, user, or role" section in [Working with inline policies \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_inline-using.html#AddingPermissions_Console) in the *IAM User Guide*\.

1. Create or identify a topic in Amazon SNS\. AWS CodeBuild uses CloudWatch Events to send build notifications to this topic through Amazon SNS\. 

   To create a topic:

   1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns](https://console.aws.amazon.com/sns)\.

   1. Choose **Create topic**\. 

   1. In **Create new topic**, for **Topic name**, enter a name for the topic \(for example, **CodeBuildDemoTopic**\)\. \(If you choose a different name, substitute it throughout this sample\.\) 

   1. Choose **Create topic**\.

   1. On the **Topic details: CodeBuildDemoTopic** page, copy the **Topic ARN** value\. You need this value for the next step\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/topic-arn.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

   For more information, see [Create a topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html) in the *Amazon SNS Developer Guide*\.

1. Subscribe one or more recipients to the topic to receive email notifications\. 

   To subscribe a recipient to a topic:

   1. With the Amazon SNS console open from the previous step, in the navigation pane, choose **Subscriptions**, and then choose **Create subscription**\.

   1. In **Create subscription**, for **Topic ARN**, paste the topic ARN you copied from the previous step\.

   1. For **Protocol**, choose **Email**\.

   1. For **Endpoint**, enter the recipient's full email address\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/create-subscription.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

   1. Choose **Create Subscription**\.

   1. Amazon SNS sends a subscription confirmation email to the recipient\. To begin receiving email notifications, the recipient must choose the **Confirm subscription** link in the subscription confirmation email\. After the recipient clicks the link, if successfully subscribed, Amazon SNS displays a confirmation message in the recipient's web browser\.

   For more information, see [Subscribe to a topic](https://docs.aws.amazon.com/sns/latest/dg/SubscribeTopic.html) in the *Amazon SNS Developer Guide*\.

1. If you are using an IAM user instead of an AWS root account or an administrator IAM user to work with CloudWatch Events, add the following statement \(between *\#\#\# BEGIN ADDING STATEMENT HERE \#\#\#* and *\#\#\# END ADDING STATEMENT HERE \#\#\#*\) to the user \(or IAM group the user is associated with\)\. Using an AWS root account is not recommended\. This statement is used to allow the user to work with CloudWatch Events\. Ellipses \(`...`\) are used for brevity and to help you locate where to add the statement\. Do not remove any statements, and do not type these ellipses into the existing policy\.

   ```
   {
     "Statement": [
       ### BEGIN ADDING STATEMENT HERE ###
       {
         "Action": [
           "events:*",
           "iam:PassRole"
         ],
         "Resource": "*",
         "Effect": "Allow"
       },
       ### END ADDING STATEMENT HERE ###
       ...
     ],
     "Version": "2012-10-17"
   }
   ```
**Note**  
The IAM entity that modifies this policy must have permission in IAM to modify policies\.  
For more information, see [Editing customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html#edit-managed-policy-console) or the "To edit or delete an inline policy for a group, user, or role" section in [Working with inline policies \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_inline-using.html#AddingPermissions_Console) in the *IAM User Guide*\.

1. Create a rule in CloudWatch Events\. To do this, open the CloudWatch console, at [https://console\.aws\.amazon\.com/cloudwatch](https://console.aws.amazon.com/cloudwatch)\.

1. In the navigation pane, under **Events**, choose **Rules**, and then choose **Create rule**\. 

1. On the **Step 1: Create rule page**, **Event Pattern** and **Build event pattern to match events by service** should already be selected\. 

1. For **Service Name**, choose **CodeBuild**\. For **Event Type**, **All Events** should already be selected\.

1. The following code should be displayed in **Event Pattern Preview**:

   ```
   {
     "source": [ 
       "aws.codebuild"
     ]
   }
   ```

   Compare your results:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/create-rule.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. Choose **Edit** and replace the code in **Event Pattern Preview** with one of the following two rule patterns\.

   This first rule pattern triggers an event when a build starts or completes for the specified build projects in AWS CodeBuild\.

   ```
   {
     "source": [ 
       "aws.codebuild"
     ], 
     "detail-type": [
       "CodeBuild Build State Change"
     ],
     "detail": {
       "build-status": [
         "IN_PROGRESS",
         "SUCCEEDED", 
         "FAILED",
         "STOPPED" 
       ],
       "project-name": [
         "my-demo-project-1",
         "my-demo-project-2"
       ]
     }  
   }
   ```

   In the preceding rule, make the following code changes as needed\.
   + To trigger an event when a build starts or completes, either leave all of the values as shown in the `build-status` array, or remove the `build-status` array altogether\. 
   + To trigger an event only when a build completes, remove `IN_PROGRESS` from the `build-status` array\. 
   + To trigger an event only when a build starts, remove all of the values except `IN_PROGRESS` from the `build-status` array\.
   + To trigger events for all build projects, remove the `project-name` array altogether\.
   + To trigger events only for individual build projects, specify the name of each build project in the `project-name` array\. 

   This second rule pattern triggers an event whenever a build moves from one build phase to another for the specified build projects in AWS CodeBuild\.

   ```
   {
     "source": [ 
       "aws.codebuild"
     ], 
     "detail-type": [
       "CodeBuild Build Phase Change" 
     ],
     "detail": {
       "completed-phase": [
         "SUBMITTED",
         "PROVISIONING",
         "DOWNLOAD_SOURCE",
         "INSTALL",
         "PRE_BUILD",
         "BUILD",
         "POST_BUILD",
         "UPLOAD_ARTIFACTS",
         "FINALIZING"
       ],
       "completed-phase-status": [
         "TIMED_OUT",
         "STOPPED",
         "FAILED", 
         "SUCCEEDED",
         "FAULT",
         "CLIENT_ERROR"
       ],
       "project-name": [
         "my-demo-project-1",
         "my-demo-project-2"
       ]
     }  
   }
   ```

   In the preceding rule, make the following code changes as needed\.
   + To trigger an event for every build phase change \(which might send up to nine notifications for each build\), either leave all of the values as shown in the `completed-phase` array, or remove the `completed-phase` array altogether\.
   + To trigger events only for individual build phase changes, remove the name of each build phase in the `completed-phase` array that you do not want to trigger an event for\.
   + To trigger an event for every build phase status change, either leave all of the values as shown in the `completed-phase-status` array, or remove the `completed-phase-status` array altogether\.
   + To trigger events only for individual build phase status changes, remove the name of each build phase status in the `completed-phase-status` array that you do not want to trigger an event for\.
   + To trigger events for all build projects, remove the `project-name` array\.
   + To trigger events for individual build projects, specify the name of each build project in the `project-name` array\. 
**Note**  
If you want to trigger events for both build state changes and build phase changes, you must create two separate rules: one for build state changes and another for build phase changes\. If you try to combine both rules into a single rule, the combined rule might produce unexpected results or stop working altogether\.

   When you have finished replacing the code, choose **Save**\.

1. For **Targets**, choose **Add target**\. 

1. In the list of targets, choose **SNS topic**\. 

1. For **Topic**, choose the topic you identified or created earlier\. 

1. Expand **Configure input**, and then choose **Input Transformer**\. 

1. In the **Input Path** box, enter one of the following input paths\.

   For a rule with a `detail-type` value of `CodeBuild Build State Change`, enter the following\.

   ```
   {"build-id":"$.detail.build-id","project-name":"$.detail.project-name","build-status":"$.detail.build-status"}
   ```

   For a rule with a `detail-type` value of `CodeBuild Build Phase Change`, enter the following\.

   ```
   {"build-id":"$.detail.build-id","project-name":"$.detail.project-name","completed-phase":"$.detail.completed-phase","completed-phase-status":"$.detail.completed-phase-status"}
   ```

   To get other types of information, see the [Build notifications input format reference](#sample-build-notifications-ref)\.

1. In the **Input Template** box, enter one of the following input templates\.

   For a rule with a `detail-type` value of `CodeBuild Build State Change`, enter the following\.

   ```
   "Build '<build-id>' for build project '<project-name>' has reached the build status of '<build-status>'."
   ```

   For a rule with a `detail-type` value of `CodeBuild Build Phase Change`, enter the following\.

   ```
   "Build '<build-id>' for build project '<project-name>' has completed the build phase of '<completed-phase>' with a status of '<completed-phase-status>'."
   ```

   Compare your results so far to the following, which shows a rule with a `detail-type` value of `CodeBuild Build State Change`:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/create-rule-2.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. Choose **Configure details**\.

1. On the **Step 2: Configure rule details** page, enter a name and an optional description\. For **State**, leave **Enabled** selected\.

   Compare your results so far to the following screen shot:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/create-rule-3.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. Choose **Create rule**\. 

1. Create build projects, run the builds, and view build information by following the steps in [Run CodeBuild directly](how-to-run.md)\.

1. Confirm that CodeBuild is now successfully sending build notifications\. For example, check to see if the build notification emails are now in your inbox\.

To change a rule's behavior, in the CloudWatch console, choose the rule you want to change, choose **Actions**, and then choose **Edit**\. Make changes to the rule, choose **Configure details**, and then choose **Update rule**\.

To stop using a rule to send build notifications, in the CloudWatch console, choose the rule you want to stop using, choose **Actions**, and then choose **Disable**\.

To delete a rule altogether, in the CloudWatch console, choose the rule you want to delete, choose **Actions**, and then choose **Delete**\.

### Related resources<a name="acb-more-info"></a>
+ For information about getting started with AWS CodeBuild, see [Getting started with AWS CodeBuild using the console](getting-started.md)\.
+ For information about troubleshooting issues in CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.
+ For information about quotas in CodeBuild, see [Quotas for AWS CodeBuild](limits.md)\.

## Build notifications input format reference<a name="sample-build-notifications-ref"></a>

CloudWatch delivers notifications in JSON format\.

Build state change notifications use the following format:

```
{
  "version": "0",
  "id": "c030038d-8c4d-6141-9545-00ff7b7153EX",
  "detail-type": "CodeBuild Build State Change",
  "source": "aws.codebuild",
  "account": "123456789012",
  "time": "2017-09-01T16:14:28Z",
  "region": "us-west-2",
  "resources":[
    "arn:aws:codebuild:us-west-2:123456789012:build/my-sample-project:8745a7a9-c340-456a-9166-edf953571bEX"
  ],
  "detail":{
    "build-status": "SUCCEEDED",
    "project-name": "my-sample-project",
    "build-id": "arn:aws:codebuild:us-west-2:123456789012:build/my-sample-project:8745a7a9-c340-456a-9166-edf953571bEX",
    "additional-information": {
      "artifact": {
        "md5sum": "da9c44c8a9a3cd4b443126e823168fEX",
        "sha256sum": "6ccc2ae1df9d155ba83c597051611c42d60e09c6329dcb14a312cecc0a8e39EX",
        "location": "arn:aws:s3:::codebuild-123456789012-output-bucket/my-output-artifact.zip"
      },
      "environment": {
        "image": "aws/codebuild/standard:4.0",
        "privileged-mode": false,
        "compute-type": "BUILD_GENERAL1_SMALL",
        "type": "LINUX_CONTAINER",
        "environment-variables": []
      },
      "timeout-in-minutes": 60,
      "build-complete": true,
      "initiator": "MyCodeBuildDemoUser",
      "build-start-time": "Sep 1, 2017 4:12:29 PM",
      "source": {
        "location": "codebuild-123456789012-input-bucket/my-input-artifact.zip",
        "type": "S3"
      },
      "logs": {
        "group-name": "/aws/codebuild/my-sample-project",
        "stream-name": "8745a7a9-c340-456a-9166-edf953571bEX",
        "deep-link": "https://console.aws.amazon.com/cloudwatch/home?region=us-west-2#logEvent:group=/aws/codebuild/my-sample-project;stream=8745a7a9-c340-456a-9166-edf953571bEX"
      },
      "phases": [
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:12:29 PM",
          "end-time": "Sep 1, 2017 4:12:29 PM",
          "duration-in-seconds": 0,
          "phase-type": "SUBMITTED",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:12:29 PM",
          "end-time": "Sep 1, 2017 4:13:05 PM",
          "duration-in-seconds": 36,
          "phase-type": "PROVISIONING",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:13:05 PM",
          "end-time": "Sep 1, 2017 4:13:10 PM",
          "duration-in-seconds": 4,
          "phase-type": "DOWNLOAD_SOURCE",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:13:10 PM",
          "end-time": "Sep 1, 2017 4:13:10 PM",
          "duration-in-seconds": 0,
          "phase-type": "INSTALL",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:13:10 PM",
          "end-time": "Sep 1, 2017 4:13:10 PM",
          "duration-in-seconds": 0,
          "phase-type": "PRE_BUILD",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:13:10 PM",
          "end-time": "Sep 1, 2017 4:14:21 PM",
          "duration-in-seconds": 70,
          "phase-type": "BUILD",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:14:21 PM",
          "end-time": "Sep 1, 2017 4:14:21 PM",
          "duration-in-seconds": 0,
          "phase-type": "POST_BUILD",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:14:21 PM",
          "end-time": "Sep 1, 2017 4:14:21 PM",
          "duration-in-seconds": 0,
          "phase-type": "UPLOAD_ARTIFACTS",
          "phase-status": "SUCCEEDED"
        },
         {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:14:21 PM",
          "end-time": "Sep 1, 2017 4:14:26 PM",
          "duration-in-seconds": 4,
          "phase-type": "FINALIZING",
          "phase-status": "SUCCEEDED"
        },
        {
          "start-time": "Sep 1, 2017 4:14:26 PM",
          "phase-type": "COMPLETED"
        }
      ]
    },
    "current-phase": "COMPLETED",
    "current-phase-context": "[]",
    "version": "1"
  }
}
```

Build phase change notifications use the following format:

```
{
  "version": "0",
  "id": "43ddc2bd-af76-9ca5-2dc7-b695e15adeEX",
  "detail-type": "CodeBuild Build Phase Change",
  "source": "aws.codebuild",
  "account": "123456789012",
  "time": "2017-09-01T16:14:21Z",
  "region": "us-west-2",
  "resources":[
    "arn:aws:codebuild:us-west-2:123456789012:build/my-sample-project:8745a7a9-c340-456a-9166-edf953571bEX"
  ],
  "detail":{
    "completed-phase": "COMPLETED",
    "project-name": "my-sample-project",
    "build-id": "arn:aws:codebuild:us-west-2:123456789012:build/my-sample-project:8745a7a9-c340-456a-9166-edf953571bEX",
    "completed-phase-context": "[]",
    "additional-information": {
      "artifact": {
        "md5sum": "da9c44c8a9a3cd4b443126e823168fEX",
        "sha256sum": "6ccc2ae1df9d155ba83c597051611c42d60e09c6329dcb14a312cecc0a8e39EX",
        "location": "arn:aws:s3:::codebuild-123456789012-output-bucket/my-output-artifact.zip"
      },
      "environment": {
        "image": "aws/codebuild/standard:4.0",
        "privileged-mode": false,
        "compute-type": "BUILD_GENERAL1_SMALL",
        "type": "LINUX_CONTAINER",
        "environment-variables": []
      },
      "timeout-in-minutes": 60,
      "build-complete": true,
      "initiator": "MyCodeBuildDemoUser",
      "build-start-time": "Sep 1, 2017 4:12:29 PM",
      "source": {
        "location": "codebuild-123456789012-input-bucket/my-input-artifact.zip",
        "type": "S3"
      },
      "logs": {
        "group-name": "/aws/codebuild/my-sample-project",
        "stream-name": "8745a7a9-c340-456a-9166-edf953571bEX",
        "deep-link": "https://console.aws.amazon.com/cloudwatch/home?region=us-west-2#logEvent:group=/aws/codebuild/my-sample-project;stream=8745a7a9-c340-456a-9166-edf953571bEX"
      },
      "phases": [
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:12:29 PM",
          "end-time": "Sep 1, 2017 4:12:29 PM",
          "duration-in-seconds": 0,
          "phase-type": "SUBMITTED",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:12:29 PM",
          "end-time": "Sep 1, 2017 4:13:05 PM",
          "duration-in-seconds": 36,
          "phase-type": "PROVISIONING",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:13:05 PM",
          "end-time": "Sep 1, 2017 4:13:10 PM",
          "duration-in-seconds": 4,
          "phase-type": "DOWNLOAD_SOURCE",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:13:10 PM",
          "end-time": "Sep 1, 2017 4:13:10 PM",
          "duration-in-seconds": 0,
          "phase-type": "INSTALL",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:13:10 PM",
          "end-time": "Sep 1, 2017 4:13:10 PM",
          "duration-in-seconds": 0,
          "phase-type": "PRE_BUILD",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:13:10 PM",
          "end-time": "Sep 1, 2017 4:14:21 PM",
          "duration-in-seconds": 70,
          "phase-type": "BUILD",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:14:21 PM",
          "end-time": "Sep 1, 2017 4:14:21 PM",
          "duration-in-seconds": 0,
          "phase-type": "POST_BUILD",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:14:21 PM",
          "end-time": "Sep 1, 2017 4:14:21 PM",
          "duration-in-seconds": 0,
          "phase-type": "UPLOAD_ARTIFACTS",
          "phase-status": "SUCCEEDED"
        },
        {
          "phase-context": [],
          "start-time": "Sep 1, 2017 4:14:21 PM",
          "end-time": "Sep 1, 2017 4:14:26 PM",
          "duration-in-seconds": 4,
          "phase-type": "FINALIZING",
          "phase-status": "SUCCEEDED"
        },
        {
          "start-time": "Sep 1, 2017 4:14:26 PM",
          "phase-type": "COMPLETED"
        }
      ]  
    },
    "completed-phase-status": "SUCCEEDED",
    "completed-phase-duration-seconds": 4,
    "version": "1",
    "completed-phase-start": "Sep 1, 2017 4:14:21 PM",
    "completed-phase-end": "Sep 1, 2017 4:14:26 PM"
  }
}
```