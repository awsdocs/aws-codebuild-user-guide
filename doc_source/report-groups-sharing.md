# Working with shared report groups<a name="report-groups-sharing"></a>

Report group sharing allows multiple AWS accounts or users to view a report group, its unexpired reports, and the test results of its reports\. In this model, the account that owns the report group \(owner\) shares a report group with other accounts \(consumers\)\. A consumer cannot edit a report group\. A report expires 30 days after it is created\.

**Topics**
+ [Prerequisites for sharing report groups](#report-groups-sharing-prereqs)
+ [Prerequisites for accessing report groups shared with you](#report-groups-sharing-access-prereqs)
+ [Related services](#report-groups-sharing-related)
+ [Sharing a report group](#report-groups-sharing-share)
+ [Unsharing a shared report group](#report-groups-sharing-unshare)
+ [Identifying a shared report group](#report-groups-sharing-identify)
+ [Shared report group permissions](#report-groups-sharing-perms)

## Prerequisites for sharing report groups<a name="report-groups-sharing-prereqs"></a>

 To share a report group, your AWS account must own it\. You cannot share a report group that has been shared with you\. 

## Prerequisites for accessing report groups shared with you<a name="report-groups-sharing-access-prereqs"></a>

To access a shared report group, a consumer's IAM role requires the `BatchGetReportGroups` permission\. You can attach the following policy to their IAM role: 

```
{
    "Effect": "Allow",
    "Resource": [
        "*"
    ],
    "Action": [
        "codebuild:BatchGetReportGroups"
    ]
}
```

 For more information, see [Using identity\-based policies for AWS CodeBuild](auth-and-access-control-iam-identity-based-access-control.md)\. 

## Related services<a name="report-groups-sharing-related"></a>

Report group sharing integrates with AWS Resource Access Manager \(AWS RAM\), a service that makes it possible for you to share your AWS resources with any AWS account or through AWS Organizations\. With AWS RAM, you share resources that you own by creating a *resource share* that specifies the resources and the consumers to share them with\. Consumers can be individual AWS accounts, organizational units in AWS Organizations, or an entire organization in AWS Organizations\.

For more information, see the *[AWS RAM User Guide](https://docs.aws.amazon.com/ram/latest/userguide/)*\.

## Sharing a report group<a name="report-groups-sharing-share"></a>

 When you share a report group, the consumer is granted read\-only access to the report group and its reports\. The consumer can use the AWS CLI to view the report group, its reports, and the test case results for each report\. The consumer cannot: 
+  View a shared report group or its reports in the CodeBuild console\. 
+  Edit a shared report group\. 
+  Use the ARN of the shared report group in a project to run a report\. A project build that specifies a shared report group fails\. 

You can use the CodeBuild console to add a report group to an existing resource share\. If you want to add the report group to a new resource share, you must first create it in the [AWS RAM console](https://console.aws.amazon.com/ram)\.

To share a report group with organizational units or an entire organization, you must enable sharing with AWS Organizations\. For more information, see [Enable sharing with AWS Organizations](https://docs.aws.amazon.com/ram/latest/userguide/getting-started-sharing.html) in the *AWS RAM User Guide*\.

You can use the CodeBuild console, AWS RAM console, or AWS CLI to share report groups that you own\.

**To share a report group that you own \(CodeBuild console\)**

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the navigation pane, choose **Report groups**\.

1.  Choose the project you want to share, and then choose **Share**\. For more information, see [Create a resource share](https://docs.aws.amazon.com/ram/latest/userguide/getting-started-sharing.html#getting-started-sharing-create) in the *AWS RAM User Guide*\. 

**To share report groups that you own \(AWS RAM console\)**  
See [Creating a resource share](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing.html#working-with-sharing-create) in the *AWS RAM User Guide*\.

**To share report groups that you own \(AWS RAM command\)**  
Use the [create\-resource\-share](https://docs.aws.amazon.com/cli/latest/reference/ram/create-resource-share.html) command\.

 **To share a report group that you own \(CodeBuild command\)** 

Use the [put\-resource\-policy](https://docs.aws.amazon.com/cli/latest/reference/codebuild/put-resource-policy.html) command:

1. Create a file named `policy.json` and copy the following into it\. 

   ```
   {
      "Version":"2012-10-17",
      "Statement":[{
        "Effect":"Allow",
        "Principal":{
          "AWS":"consumer-aws-account-id-or-user"
        },
        "Action":[
          "codebuild:BatchGetReportGroups",
          "codebuild:BatchGetReports",
          "codebuild:ListReportsForReportGroup",
          "codebuild:DescribeTestCases"],
        "Resource":"arn-of-report-group-to-share"
      }]
    }
   ```

1. Update `policy.json` with the report group ARN and identifiers to share it with\. The following example grants read\-only access to the report group with the ARN `arn:aws:codebuild:us-west-2:123456789012:report-group/my-report-group` to Alice and the root user for the AWS account identified by 123456789012\. 

   ```
   {
      "Version":"2012-10-17",
      "Statement":[{
        "Effect":"Allow",
        "Principal":{
          "AWS": [
             "arn:aws:iam:123456789012:user/Alice",
             "123456789012"
           ]
        },
        "Action":[
          "codebuild:BatchGetReportGroups",
          "codebuild:BatchGetReports",
          "codebuild:ListReportsForReportGroup",
          "codebuild:DescribeTestCases"],
        "Resource":"arn:aws:codebuild:us-west-2:123456789012:report-group/my-report-group"
      }]
    }
   ```

1. Run the following command\. 

   ```
   aws codebuild put-resource-policy --resource-arn report-group-arn --policy file://policy.json
   ```

## Unsharing a shared report group<a name="report-groups-sharing-unshare"></a>

An unshared report group, including its reports and their test case results, can be accessed only by its owner\. If you unshare a report group, any AWS account or user you previously shared it with cannot access the report group, its reports, or the results of test cases in the reports\.

To unshare a shared report group that you own, you must remove it from the resource share\. You can use the AWS RAM console or AWS CLI to do this\.

**To unshare a shared report group that you own \(AWS RAM console\)**  
See [Updating a resource share](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing.html#working-with-sharing-update) in the *AWS RAM User Guide*\.

**To unshare a shared report group that you own \(AWS RAM command\)**  
Use the [disassociate\-resource\-share](https://docs.aws.amazon.com/cli/latest/reference/ram/disassociate-resource-share.html) command\.

 ** To unshare report group that you own CodeBuild command\)** 

Run the [delete\-resource\-policy](https://docs.aws.amazon.com/cli/latest/reference/codebuild/delete-resource-policy.html) command and specify the ARN of the report group you want to unshare:

```
aws codebuild delete-resource-policy --resource-arn report-group-arn
```

## Identifying a shared report group<a name="report-groups-sharing-identify"></a>

Owners and consumers can use the AWS CLI to identify shared report groups\. 

To identify and get information about a shared report group and its reports, use the following commands: 
+  To see the ARNs of report groups shared with you, run `[list\-shared\-report\-groups](https://docs.aws.amazon.com/cli/latest/reference/codebuild/list-shared-report-groups.html)`: 

  ```
  aws codebuild list-shared-report-groups
  ```
+  To see the ARNs of the reports in a report group, run `[list\-reports\-for\-report\-group](https://docs.aws.amazon.com/cli/latest/reference/codebuild/list-reports-for-report-group.html)` using the report group ARN: 

  ```
  aws codebuild list-reports-for-report-group --report-group-arn report-group-arn
  ```
+  To see information about test cases in a report, run `[describe\-test\-cases](https://docs.aws.amazon.com/cli/latest/reference/codebuild/describe-test-cases.html)` using the report ARN: 

  ```
  aws codebuild describe-test-cases --report-arn report-arn
  ```

   The output looks like the following: 

  ```
  {
      "testCases": [
          {
              "status": "FAILED",
              "name": "Test case 1",
              "expired": 1575916770.0,
              "reportArn": "report-arn",
              "prefix": "Cucumber tests for agent",
              "message": "A test message",
              "durationInNanoSeconds": 1540540,
              "testRawDataPath": "path-to-output-report-files"
          },
          {
              "status": "SUCCEEDED",
              "name": "Test case 2",
              "expired": 1575916770.0,
              "reportArn": "report-arn",
              "prefix": "Cucumber tests for agent",
              "message": "A test message",
              "durationInNanoSeconds": 1540540,
              "testRawDataPath": "path-to-output-report-files"
          }
      ]
  }
  ```

## Shared report group permissions<a name="report-groups-sharing-perms"></a>

### Permissions for owners<a name="report-groups-perms-owner"></a>

A report group owner can edit the report group and specify it in a project to run reports\.

### Permissions for consumers<a name="report-groups-perms-consumer"></a>

A report group consumer can view a report group, its reports, and the test case results for its reports\. A consumer cannot edit a report group or its reports, and cannot use it to create reports\.