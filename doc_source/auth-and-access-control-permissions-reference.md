# AWS CodeBuild Permissions Reference<a name="auth-and-access-control-permissions-reference"></a>

You can use the following table as a reference when you are setting up [Access Control](auth-and-access-control.md#access-control) and writing permissions policies that you can attach to an IAM identity \(identity\-based policies\)\. 

You can use AWS\-wide condition keys in your AWS CodeBuild policies to express conditions\. For a list, see [Available Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.

You specify the actions in the policy's `Action` field\. To specify an action, use the `codebuild:` prefix followed by the API operation name \(for example, `codebuild:CreateProject` and `codebuild:StartBuild`\)\. To specify multiple actions in a single statement, separate them with commas \(for example, `"Action": [ "codebuild:CreateProject", "codebuild:StartBuild" ]`\)\.

**Using Wildcard Characters**

You specify an ARN, with or without a wildcard character \(\*\), as the resource value in the policy's `Resource` field\. You can use a wildcard to specify multiple actions or resources\. For example, `codebuild:*` specifies all AWS CodeBuild actions and `codebuild:Batch*` specifies all AWS CodeBuild actions that begin with the word `Batch`\. The following example grants access to all build project with names that begin with `my`: 

```
arn:aws:codebuild:us-east-2:123456789012:project/my*
```

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**AWS CodeBuild API Operations and Required Permissions for Actions**  

| AWS CodeBuild API operations | Required permissions \(API actions\) | Resources | 
| --- | --- | --- | 
| BatchDeleteBuilds |  `codebuild:BatchDeleteBuilds` Required to delete builds\.  |  `arn:aws:codebuild:region-ID:account-ID:project/project-name`  | 
| BatchGetBuilds |  `codebuild:BatchGetBuilds` Required to get information about builds\.  |  `arn:aws:codebuild:region-ID:account-ID:project/project-name`  | 
| BatchGetProjects |  `codebuild:BatchGetProjects` Required to get information about build projects\.  |  `arn:aws:codebuild:region-ID:account-ID:project/project-name`  | 
| CreateProject |  `codebuild:CreateProject` `iam:PassRole` Required to create build projects\.  |  `arn:aws:codebuild:region-ID:account-ID:project/project-name` `arn:aws:iam:account-ID:role/role-name`  | 
| DeleteProject |  `codebuild:DeleteProject` Required to delete build projects\.  |  `arn:aws:codebuild:region-ID:account-ID:project/project-name`  | 
| ListBuilds | codebuild:ListBuildsRequired to get a list of build IDs\. |  `*`  | 
| ListBuildsForProject |  `codebuild:ListBuildsForProject` Required to get a list of build IDs for a build project\.  |  `arn:aws:codebuild:region-ID:account-ID:project/project-name`  | 
| ListCuratedEnvironmentImages |  `codebuild:ListCuratedEnvironmentImages` Required to get information about all Docker images that are managed by AWS CodeBuild\.  |  `*` \(required, but does not refer to an addressable AWS resource\)  | 
| ListProjects |  `codebuild:ListProjects` Required to get a list of build project names\.  |  `*`  | 
| StartBuild |  `codebuild:StartBuild` Required to start running builds\.  |  `arn:aws:codebuild:region-ID:account-ID:project/project-name`  | 
| StopBuild |  `codebuild:StopBuild` Required to attempt to stop running builds\.  |  `arn:aws:codebuild:region-ID:account-ID:project/project-name`  | 
| UpdateProject |  `codebuild:UpdateProject` `iam:PassRole` Required to change information about builds\.  |  `arn:aws:codebuild:region-ID:account-ID:project/project-name` `arn:aws:iam:account-ID:role/role-name`  | 