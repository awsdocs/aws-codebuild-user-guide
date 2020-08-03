# AWS CodeBuild permissions reference<a name="auth-and-access-control-permissions-reference"></a>

You can use the following table as a reference when you are setting up [Access control](auth-and-access-control.md#access-control) and writing permissions policies that you can attach to an IAM identity \(identity\-based policies\)\. 

You can use AWS\-wide condition keys in your AWS CodeBuild policies to express conditions\. For a list, see [Available Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.

You specify the actions in the policy's `Action` field\. To specify an action, use the `codebuild:` prefix followed by the API operation name \(for example, `codebuild:CreateProject` and `codebuild:StartBuild`\)\. To specify multiple actions in a single statement, separate them with commas \(for example, `"Action": [ "codebuild:CreateProject", "codebuild:StartBuild" ]`\)\.

**Using Wildcard Characters**

You specify an ARN, with or without a wildcard character \(\*\), as the resource value in the policy's `Resource` field\. You can use a wildcard to specify multiple actions or resources\. For example, `codebuild:*` specifies all CodeBuild actions and `codebuild:Batch*` specifies all CodeBuild actions that begin with the word `Batch`\. The following example grants access to all build project with names that begin with `my`: 

```
arn:aws:codebuild:us-east-2:123456789012:project/my*
```<a name="actions-related-to-objects-table"></a>CodeBuild API operations and required permissions for actions

BatchDeleteBuilds  
 **Action:** `codebuild:BatchDeleteBuilds`   
Required to delete builds\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:project/project-name ` 

BatchGetBuilds  
 **Action:** `codebuild:BatchGetBuilds`   
Required to get information about builds\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:project/project-name ` 

BatchGetProjects  
 **Action:** `codebuild:BatchGetProjects`   
Required to get information about build projects\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:project/project-name ` 

BatchGetReportGroups  
 **Action:** `codebuild:BatchGetReportGroups`   
Required to get information about report groups\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:report-group/report-group-name ` 

BatchGetReports  
 **Action:** `codebuild:BatchGetReports`   
Required to get information about reports\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:report-group/report-group-name ` 

BatchPutTestCases ¹  
 **Action:** `codebuild:BatchPutTestCases`   
Required to create or update a test report\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:report-group/report-group-name ` 

CreateProject  
 **Actions:** `codebuild:CreateProject`, `iam:PassRole`   
Required to create build projects\.  
 **Resources:**   
+  `arn:aws:codebuild:region-ID:account-ID:project/project-name ` 
+  `arn:aws:iam:account-ID:role/role-name ` 

CreateReport ¹  
 **Action:** `codebuild:CreateReport`   
Required to create a test report\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:report-group/report-group-name ` 

CreateReportGroup  
 **Action:** `codebuild:CreateReportGroup`   
Required to create a report group\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:report-group/report-group-name ` 

CreateWebhook  
 **Action:** `codebuild:CreateWebhook`   
Required to create a webhook\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:project/project-name ` 

DeleteReport  
 **Action:** `codebuild:DeleteReport`   
Required to delete a report\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:report-group/report-group-name ` 

DeleteReportGroup  
 **Action:** `codebuild:DeleteReportGroup`   
Required to delete a report group\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:report-group/report-group-name ` 

DeleteSourceCredentials  
 **Action:** `codebuild:DeleteSourceCredentials`   
Required to delete a set of `SourceCredentialsInfo` objects that contain information about credentials for a GitHub, GitHub Enterprise Server, or Bitbucket repository\.   
 **Resource:** `*` 

DeleteWebhook  
 **Action:** `codebuild:DeleteWebhook`   
Required to create a webhook\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:project/project-name ` 

DescribeTestCases  
 **Action:** `codebuild:DescribeTestCases`   
Required to return a paginated list of test cases\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:report-group/report-group-name ` 

ImportSourceCredentials  
 **Action:** `codebuild:ImportSourceCredentials`   
Required to import a set of `SourceCredentialsInfo` objects that contain information about credentials for a GitHub, GitHub Enterprise Server, or Bitbucket repository\.   
 **Resource:** `*` 

InvalidateProjectCache  
 **Action:** `codebuild:InvalidateProjectCache`   
Required to reset the cache for a project\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:project/project-name ` 

ListBuilds  
 **Action:** `codebuild:ListBuilds`   
Required to get a list of build IDs\.  
 **Resource:** `*` 

ListBuildsForProject  
 **Action:** `codebuild:ListBuildsForProject`   
Required to get a list of build IDs for a build project\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:project/project-name ` 

ListCuratedEnvironmentImages  
 **Action:** `codebuild:ListCuratedEnvironmentImages`   
Required to get information about all Docker images that are managed by AWS CodeBuild\.   
 **Resource:** `*` \(required, but does not refer to an addressable AWS resource\) 

ListProjects  
 **Action:** `codebuild:ListProjects`   
Required to get a list of build project names\.  
 **Resource:** `*` 

ListReportGroups  
 **Action:** `codebuild:ListReportGroups`   
Required to get a list of report groups\.  
 **Resource:** `*` 

ListReports  
 **Action:** `codebuild:ListReports`   
Required to get a list of reports\.  
 **Resource:** `*` 

ListReportsForReportGroup  
 **Action:** `codebuild:ListReportsForReportGroup`   
Required to get a list of reports for a report group\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:report-group/report-group-name ` 

RetryBuild  
**Action:** `codebuild:RetryBuild`   
Required to retry builds\.  
**Resource:** `arn:aws:codebuild:region-ID:account-ID:project/project-name `

StartBuild  
 **Action:** `codebuild:StartBuild`   
Required to start running builds\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:project/project-name ` 

StopBuild  
 **Action:** `codebuild:StopBuild`   
Required to attempt to stop running builds\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:project/project-name ` 

UpdateProject  
 **Actions:** `codebuild:UpdateProject`, `iam:PassRole`   
Required to change information about builds\.  
 **Resources:**   
+  `arn:aws:codebuild:region-ID:account-ID:project/project-name ` 
+  `arn:aws:iam:account-ID:role/role-name ` 

UpdateReport ¹  
 **Action:** `codebuild:UpdateReport`   
Required to create or update a test report\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:report-group/report-group-name ` 

UpdateReportGroup  
 **Action:** `codebuild:UpdateReportGroup`   
Required to update a report group\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:report-group/report-group-name ` 

UpdateWebhook  
 **Action:** `codebuild:UpdateWebhook`   
Required to update a webhook\.  
 **Resource:** `arn:aws:codebuild:region-ID:account-ID:project/project-name ` 

¹ Used for permission only\. There is no API for this action\.