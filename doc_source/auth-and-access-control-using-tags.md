# Using tags to control access to AWS CodeBuild resources<a name="auth-and-access-control-using-tags"></a>

Conditions in IAM policy statements are part of the syntax that you can use to specify permissions to CodeBuild project\-based actions\. You can create a policy that allows or denies actions on projects based on the tags associated with those projects, and then apply those policies to the IAM groups you configure for managing IAM users\. For information about applying tags to a project using the console or AWS CLI, see [Create a build project in AWS CodeBuild](create-project.md)\. For information about applying tags using the CodeBuild SDK, see [CreateProject ](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateProject.html#API_CreateProject_RequestSyntax) and [Tags](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_Tag.html) in the *CodeBuild API Reference*\. For information about using tags to control access to AWS resources, see [Controlling Access to AWS Resources Using Resource Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the *IAM User Guide*\.

**Example 1: Limit CodeBuild project actions based on resource tags**  
 The following example denies all `BatchGetProjects` actions on projects tagged with the key `Environment` with the key value of `Production`\. A user's administrator must attach this IAM policy in addition to the managed user policy to unauthorized IAM users\. The `aws:ResourceTag` condition key is used to control access to resources based on their tags\.   

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "codebuild:BatchGetProjects"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:ResourceTag/Environment": "Production"
        }
      }
    }
  ]
}
```

**Example 2: Limit CodeBuild project actions based on request tags**  
The following policy denies users permission to the `CreateProject` action if the request contains a tag with the key `Environment` and the key value `Production`\. In addition, the policy prevents these unauthorized users from modifying projects by using the `aws:TagKeys` condition key to not allow `UpdateProject` if the request contains a tag with the key `Environment`\. An administrator must attach this IAM policy in addition to the managed user policy to users who are not authorized to perform these actions\. The `aws:RequestTag` condition key is used to control which tags can be passed in an IAM request  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "codebuild:CreateProject"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:RequestTag/Environment": "Production"
        }
      }
    },
    {
      "Effect": "Deny",
      "Action": [
        "codebuild:UpdateProject"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:TagKeys": ["Environment"]
        }
      }
    }
  ]
}
```

**Example 3: Deny or allow actions on report groups based on resource tags**  
You can create a policy that allows or denies actions on CodeBuild resources \(projects and report groups\) based on the AWS tags associated with those resources, and then apply those policies to the IAM groups you configure for managing IAM users\. For example, you can create a policy that denies all CodeBuild actions on any report group with the AWS tag key `Status` and the key value of `Secret`, and then apply that policy to the IAM group you created for general developers \(*Developers*\)\. You then need to make sure that the developers working on those tagged report groups are not members of that general *Developers* group, but belong instead to a different IAM group that does not have the restrictive policy applied \(`SecretDevelopers`\)\.  
The following example denies all CodeBuild actions on report groups tagged with the key `Status` and the key value of `Secret`:  

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Deny",
      "Action" : [
        "codebuild:BatchGetReportGroups,"
        "codebuild:CreateReportGroup",
        "codebuild:DeleteReportGroup",
        "codebuild:ListReportGroups",
        "codebuild:ListReportsForReportGroup",
        "codebuild:UpdateReportGroup"
       ]
      "Resource" : "*",
      "Condition" : {
         "StringEquals" : "aws:ResourceTag/Status": "Secret"
        }
    }
  ]
}
```

**Example 4: Limit CodeBuild actions to AWSCodeBuildDeveloperAccess based on resource tags**  
You can create policies that allow CodeBuild actions on all report groups and projects that are not tagged with specific tags\. For example, the following policy allows the equivalent of [AWSCodeBuildDeveloperAccess](auth-and-access-control-iam-identity-based-access-control.md#developer-access-policy) permissions for all report groups and projects except those tagged with the specified tags:  

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Action": [
            "codebuild:StartBuild",
            "codebuild:StopBuild",
            "codebuild:BatchGet*",
            "codebuild:GetResourcePolicy",
            "codebuild:DescribeTestCases",
            "codebuild:List*",
            "codecommit:GetBranch",
            "codecommit:GetCommit",
            "codecommit:GetRepository",
            "codecommit:ListBranches",
            "cloudwatch:GetMetricStatistics",
            "events:DescribeRule",
            "events:ListTargetsByRule",
            "events:ListRuleNamesByTarget",
            "logs:GetLogEvents",
            "s3:GetBucketLocation",
            "s3:ListAllMyBuckets"
         ],
         "Resource": "*",
         "Condition": {
            "StringNotEquals": {
               "aws:ResourceTag/Status": "Secret",
               "aws:ResourceTag/Team": "Saanvi"
            }
         }
      }
   ]
}
```