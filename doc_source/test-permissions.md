# Working with Test Report Permissions<a name="test-permissions"></a>


|  | 
| --- |
| The test reporting feature is in preview release for CodeBuild and is subject to change\. | 

 This topic describes important information about permissions related to test reporting\. 

**Topics**
+ [Create a Role for Test Reports](#test-permissions-required)
+ [Permissions for Test Reporting Operations](#test-permissions-related-to-reporting)
+ [Test Reporting Permissions Examples](#test-permissions-examples)

## Create a Role for Test Reports<a name="test-permissions-required"></a>


|  | 
| --- |
| The test reporting feature is in preview release for CodeBuild and is subject to change\. | 

 To run a test report, and to update a project to include test reports, your IAM role requires the following permissions\. These permissions are included in the predefined AWS managed policies\. If you want to add test reporting to an existing build project, you must add these permissions yourself\.
+  `CreateReportGroup` 
+  `CreateReport` 
+  `UpdateReport` 
+  `BatchPutTestCases` 

**Note**  
 `BatchPutTestCases`, `CreateReport`, and `UpdateReport` are not public permissions\. You cannot call a corresponding AWS CLI command or SDK method for these permissions\. 

 To make sure you have these permissions, you can attach the following policy to your IAM role: 

```
{
    "Effect": "Allow",
    "Resource": [
        "*"
    ],
    "Action": [
        "codebuild:CreateReportGroup",
        "codebuild:CreateReport",
        "codebuild:UpdateReport",
        "codebuild:BatchPutTestCases"
    ]
}
```

 We recommend that you restrict this policy to only those report groups you must use\. The following restricts permissions to only the report groups with the two ARNs in the policy: 

```
{
    "Effect": "Allow",
    "Resource": [
        "arn:aws:codebuild:your-region:your-aws-account-id:report-group/report-group-name-1",
        "arn:aws:codebuild:your-region:your-aws-account-id:report-group/report-group-name-2"
    ],
    "Action": [
        "codebuild:CreateReportGroup",
        "codebuild:CreateReport",
        "codebuild:UpdateReport",
        "codebuild:BatchPutTestCases"
    ]
}
```

 The following restricts permissions to only report groups created by running builds of a project named `my-project`: 

```
{
    "Effect": "Allow",
    "Resource": [
        "arn:aws:codebuild:your-region:your-aws-account-id:report-group/my-project-*"
    ],
    "Action": [
        "codebuild:CreateReportGroup",
        "codebuild:CreateReport",
        "codebuild:UpdateReport",
        "codebuild:BatchPutTestCases"
    ]
}
```

## Permissions for Test Reporting Operations<a name="test-permissions-related-to-reporting"></a>


|  | 
| --- |
| The test reporting feature is in preview release for CodeBuild and is subject to change\. | 

 You can specify permissions for the following test reporting CodeBuild API operations: 
+  `BatchGetReportGroups` 
+  `BatchGetReports` 
+  `CreateReportGroup` 
+  `DeleteReportGroup` 
+  `DeleteReport` 
+  `DescribeTestCases` 
+  `ListReportGroups` 
+  `ListReports` 
+  `ListReportsForReportGroup` 
+  `UpdateReportGroup` 

For more information, see [CodeBuild Permissions Reference](auth-and-access-control-permissions-reference.md)\.

## Test Reporting Permissions Examples<a name="test-permissions-examples"></a>


|  | 
| --- |
| The test reporting feature is in preview release for CodeBuild and is subject to change\. | 

 For information about sample policies related to test reporting, see the following: 
+  [Allow a User to Get Information About Report Groups](auth-and-access-control-iam-identity-based-access-control.md#customer-managed-policies-get-information-about-report-group) 
+  [Allow a User to Get Information About Reports](auth-and-access-control-iam-identity-based-access-control.md#customer-managed-policies-get-information-about-reports) 
+  [Allow a User to Create a Report Group](auth-and-access-control-iam-identity-based-access-control.md#customer-managed-policies-example-create-report-group) 
+  [Allow a User to Delete a Report Group](auth-and-access-control-iam-identity-based-access-control.md#customer-managed-policies-example-delete-report-group) 
+  [Allow a User to Delete a Report](auth-and-access-control-iam-identity-based-access-control.md#customer-managed-policies-example-delete-report) 
+  [Allow a User to Get a List of Test Cases for a Report](auth-and-access-control-iam-identity-based-access-control.md#customer-managed-policies-example-get-list-of-test-cases-for-report) 
+  [Allow a User to Get a List of Report Groups](auth-and-access-control-iam-identity-based-access-control.md#customer-managed-policies-example-get-list-of-report-groups) 
+  [Allow a User to Get a List of Reports](auth-and-access-control-iam-identity-based-access-control.md#customer-managed-policies-example-get-list-of-reports) 
+  [Allow a User to Get a List of Reports for a Report Group](auth-and-access-control-iam-identity-based-access-control.md#customer-managed-policies-example-get-list-of-reports-for-report-group) 
+  [Allow a User to Change a Report Group](auth-and-access-control-iam-identity-based-access-control.md#customer-managed-policies-example-change-report-group) 