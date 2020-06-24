# Using identity\-based policies for AWS CodeBuild<a name="auth-and-access-control-iam-identity-based-access-control"></a>

This topic provides examples of identity\-based policies that demonstrate how an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\) and thereby grant permissions to perform operations on AWS CodeBuild resources\.

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your CodeBuild resources\. For more information, see [Overview of managing access permissions to your AWS CodeBuild resources](auth-and-access-control-iam-access-control-identity-based.md)\.

**Topics**
+ [Permissions required to use the AWS CodeBuild console](#console-permissions)
+ [Permissions required for the AWS CodeBuild console to connect to source providers](#console-policies)
+ [AWS managed \(predefined\) policies for AWS CodeBuild](#managed-policies)
+ [CodeBuild managed policies and notifications](#notifications-permissions)
+ [Customer\-managed policy examples](#customer-managed-policies)

The following shows an example of a permissions policy that allows a user to get information about build projects only in the `us-east-2` region for account `123456789012` for any build project that starts with the name `my`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:BatchGetProjects",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:project/my*"
    }
  ]
}
```

## Permissions required to use the AWS CodeBuild console<a name="console-permissions"></a>

A user who uses the AWS CodeBuild console must have a minimum set of permissions that allows the user to describe other AWS resources for the AWS account\. You must have permissions from the following services:
+ AWS CodeBuild
+ Amazon CloudWatch
+ CodeCommit \(if you are storing your source code in an AWS CodeCommit repository\)
+ Amazon Elastic Container Registry \(Amazon ECR\) \(if you are using a build environment that relies on a Docker image in an Amazon ECR repository\)
+ Amazon Elastic Container Service \(Amazon ECS\) \(if you are using a build environment that relies on a Docker image in an Amazon ECR repository\)
+ AWS Identity and Access Management \(IAM\)
+ AWS Key Management Service \(AWS KMS\)
+ Amazon Simple Storage Service \(Amazon S3\)

If you create an IAM policy that is more restrictive than the minimum required permissions, the console won't function as intended\.

## Permissions required for the AWS CodeBuild console to connect to source providers<a name="console-policies"></a>

The AWS CodeBuild console uses the following API actions to connect to source providers \(for example, GitHub repositories\)\.
+ `codebuild:ListConnectedOAuthAccounts`
+ `codebuild:ListRepositories`
+ `codebuild:PersistOAuthToken`
+ `codebuild:ImportSourceCredentials`

You can associate source providers \(such as GitHub repositories\) with your build projects using the AWS CodeBuild console\. To do this, you must first add the preceding API actions to IAM access policies associated with the IAM user you use to access the AWS CodeBuild console\.

The `ListConnectedOAuthAccounts`, `ListRepositories`, and `PersistOAuthToken` API actions are not intended to be called by your code\. Therefore, these API actions are not included in the AWS CLI and AWS SDKs\.

## AWS managed \(predefined\) policies for AWS CodeBuild<a name="managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so you can avoid having to investigate what permissions are needed\. The managed policies for CodeBuild also provide permissions to perform operations in other serivces, such as IAM, AWS CodeCommit,Amazon EC2, Amazon ECR, Amazon SNS, and Amazon CloudWatch Events, as required for the responsibilities for the users who have been granted the policy in question\. For example, the `AWSCodeBuildAdminAccess` policy is an administrative\-level user policy that allows users with this policy to create and manage CloudWatch Events rules for project builds and Amazon SNS topics for notifications about project\-related events \(topics whose names are prefixed with `arn:aws:codebuild:`\), as well as administer projects and report groups in CodeBuild\. For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

The following AWS managed policies, which you can attach to users in your account, are specific to AWS CodeBuild\.
+ `AWSCodeBuildAdminAccess` – Provides full access to CodeBuild including permissions to administrate CodeBuild build projects\. 
+ `AWSCodeBuildDeveloperAccess` – Provides access to CodeBuild but does not allow build project administration\.
+ `AWSCodeBuildReadOnlyAccess` – Provides read\-only access to CodeBuild\.

To access build output artifacts that CodeBuild creates, you must also attach the AWS managed policy named `AmazonS3ReadOnlyAccess`\.

To create and manage CodeBuild service roles, you must also attach the AWS managed policy named `IAMFullAccess`\.

You can also create your own custom IAM policies to allow permissions for CodeBuild actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\.

**Topics**
+ [AWSCodeBuildAdminAccess](#admin-access-policy)
+ [AWSCodeBuildDeveloperAccess](#developer-access-policy)
+ [AWSCodeBuildReadOnlyAccess](#read-only-access-policy)

### AWSCodeBuildAdminAccess<a name="admin-access-policy"></a>

`AWSCodeBuildAdminAccess` – Provides full access to CodeBuild, including permissions to administer CodeBuild build projects\. Apply this policy only to administrative\-level users to grant them full control over CodeBuild projects, report groups, and related resources in your AWS account, including the ability to delete projects and report groups\.

The `AWSCodeBuildAdminAccess` policy contains the following policy statement:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "codebuild:*",
                "codecommit:GetBranch",
                "codecommit:GetCommit",
                "codecommit:GetRepository",
                "codecommit:ListBranches",
                "codecommit:ListRepositories",
                "cloudwatch:GetMetricStatistics",
                "ec2:DescribeVpcs",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSubnets",
                "ecr:DescribeRepositories",
                "ecr:ListImages",
                "events:DeleteRule",
                "events:DescribeRule",
                "events:DisableRule",
                "events:EnableRule",
                "events:ListTargetsByRule",
                "events:ListRuleNamesByTarget",
                "events:PutRule",
                "events:PutTargets",
                "events:RemoveTargets",
                "logs:GetLogEvents",
                "s3:GetBucketLocation",
                "s3:ListAllMyBuckets"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Action": [
                "logs:DeleteLogGroup"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:logs:*:*:log-group:/aws/codebuild/*:log-stream:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:PutParameter"
            ],
            "Resource": "arn:aws:ssm:*:*:parameter/CodeBuild/*"
        },
        {
            "Sid": "CodeStarNotificationsReadWriteAccess",
            "Effect": "Allow",
            "Action": [
                "codestar-notifications:CreateNotificationRule",
                "codestar-notifications:DescribeNotificationRule",
                "codestar-notifications:UpdateNotificationRule",
                "codestar-notifications:DeleteNotificationRule",
                "codestar-notifications:Subscribe",
                "codestar-notifications:Unsubscribe"
            ],
            "Resource": "*",
            "Condition": {
                "StringLike": {
                    "codestar-notifications:NotificationsForResource": "arn:aws:codebuild:*"
                }
            }
        },
        {
            "Sid": "CodeStarNotificationsListAccess",
            "Effect": "Allow",
            "Action": [
                "codestar-notifications:ListNotificationRules",
                "codestar-notifications:ListEventTypes",
                "codestar-notifications:ListTargets",
                "codestar-notifications:ListTagsforResource"
            ],
            "Resource": "*"
        },
        {
            "Sid": "CodeStarNotificationsSNSTopicCreateAccess",
            "Effect": "Allow",
            "Action": [
                "sns:CreateTopic",
                "sns:SetTopicAttributes"
            ],
            "Resource": "arn:aws:sns:*:*:codestar-notifications*"
        },
        {
            "Sid": "SNSTopicListAccess",
            "Effect": "Allow",
            "Action": [
                "sns:ListTopics",
                "sns:GetTopicAttributes"
            ],
            "Resource": "*"
        }
    ]
}
```

### AWSCodeBuildDeveloperAccess<a name="developer-access-policy"></a>

`AWSCodeBuildDeveloperAccess` – Allows access to all of the functionality of CodeBuild and project and report group\-related resources\. This policy does not allow users to delete CodeBuild projects or report groups, or related resources in other AWS services, such as CloudWatch Events\. We recommend that you apply this policy to most users\.

The `AWSCodeBuildDeveloperAccess` policy contains the following policy statement:

```
{
    "Statement": [
        {
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
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:PutParameter"
            ],
            "Resource": "arn:aws:ssm:*:*:parameter/CodeBuild/*"
        },
        {
            "Sid": "CodeStarNotificationsReadWriteAccess",
            "Effect": "Allow",
            "Action": [
                "codestar-notifications:CreateNotificationRule",
                "codestar-notifications:DescribeNotificationRule",
                "codestar-notifications:UpdateNotificationRule",
                "codestar-notifications:Subscribe",
                "codestar-notifications:Unsubscribe"
            ],
            "Resource": "*",
            "Condition": {
                "StringLike": {
                    "codestar-notifications:NotificationsForResource": "arn:aws:codebuild:*"
                }
            }
        },
        {
            "Sid": "CodeStarNotificationsListAccess",
            "Effect": "Allow",
            "Action": [
                "codestar-notifications:ListNotificationRules",
                "codestar-notifications:ListEventTypes",
                "codestar-notifications:ListTargets",
                "codestar-notifications:ListTagsforResource"
            ],
            "Resource": "*"
        },
        {
            "Sid": "SNSTopicListAccess",
            "Effect": "Allow",
            "Action": [
                "sns:ListTopics",
                "sns:GetTopicAttributes"
            ],
            "Resource": "*"
        }
    ],
    "Version": "2012-10-17"
}
```

### AWSCodeBuildReadOnlyAccess<a name="read-only-access-policy"></a>

`AWSCodeBuildReadOnlyAccess` – Grants read\-only access to CodeBuild and related resources in other AWS services\. Apply this policy to users who can view and run builds, view projects, and view report groups, but cannot make any changes to them\. 

The `AWSCodeBuildReadOnlyAccess` policy contains the following policy statement:

```
{
    "Statement": [
        {
            "Action": [
                "codebuild:BatchGet*",
                "codebuild:GetResourcePolicy",
                "codebuild:List*",
                "codebuild:DescribeTestCases",
                "codecommit:GetBranch",
                "codecommit:GetCommit",
                "codecommit:GetRepository",
                "cloudwatch:GetMetricStatistics",
                "events:DescribeRule",
                "events:ListTargetsByRule",
                "events:ListRuleNamesByTarget",
                "logs:GetLogEvents"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Sid": "CodeStarNotificationsPowerUserAccess",
            "Effect": "Allow",
            "Action": [
                "codestar-notifications:DescribeNotificationRule"
            ],
            "Resource": "*",
            "Condition": {
                "StringLike": {
                    "codestar-notifications:NotificationsForResource": "arn:aws:codebuild:*"
                }
            }
        },
        {
            "Sid": "CodeStarNotificationsListAccess",
            "Effect": "Allow",
            "Action": [
                "codestar-notifications:ListNotificationRules",
                "codestar-notifications:ListEventTypes"
            ],
            "Resource": "*"
        }
    ],
    "Version": "2012-10-17"
}
```

## CodeBuild managed policies and notifications<a name="notifications-permissions"></a>

CodeBuild supports notifications, which can notify users of important changes to build projects\. Managed policies for CodeBuild include policy statements for notification functionality\. For more information, see [What are notifications?](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/welcome.html)\.

### Permissions related to notifications in full access managed policies<a name="notifications-fullaccess"></a>

The `AWSCodeBuildFullAccess` managed policy includes the following statements to allow full access to notifications\. Users with this managed policy applied can also create and manage Amazon SNS topics for notifications, subscribe and unsubscribe users to topics, list topics to choose as targets for notification rules, and list AWS Chatbot clients configured for Slack\.

```
    {
        "Sid": "CodeStarNotificationsReadWriteAccess",
        "Effect": "Allow",
        "Action": [
            "codestar-notifications:CreateNotificationRule",
            "codestar-notifications:DescribeNotificationRule",
            "codestar-notifications:UpdateNotificationRule",
            "codestar-notifications:DeleteNotificationRule",
            "codestar-notifications:Subscribe",
            "codestar-notifications:Unsubscribe"
        ],
        "Resource": "*",
        "Condition" : {
            "StringLike" : {"codestar-notifications:NotificationsForResource" : "arn:aws:codebuild:*"} 
        }
    },    
    {
        "Sid": "CodeStarNotificationsListAccess",
        "Effect": "Allow",
        "Action": [
            "codestar-notifications:ListNotificationRules",
            "codestar-notifications:ListTargets",
            "codestar-notifications:ListTagsforResource",
            "codestar-notifications:ListEventTypes"
        ],
        "Resource": "*"
    },
    {
        "Sid": "CodeStarNotificationsSNSTopicCreateAccess",
        "Effect": "Allow",
        "Action": [
            "sns:CreateTopic",
            "sns:SetTopicAttributes"
        ],
        "Resource": "arn:aws:sns:*:*:codestar-notifications*"
    },
    {
        "Sid": "SNSTopicListAccess",
        "Effect": "Allow",
        "Action": [
            "sns:ListTopics"
        ],
        "Resource": "*"
    },
    {
        "Sid": "CodeStarNotificationsChatbotAccess",
        "Effect": "Allow",
        "Action": [
            "chatbot:DescribeSlackChannelConfigurations"
          ],
       "Resource": "*"
    }
```

### Permissions related to notifications in read\-only managed policies<a name="notifications-readonly"></a>

The `AWSCodeBuildReadOnlyAccess` managed policy includes the following statements to allow read\-only access to notifications\. Users with this managed policy applied can view notifications for resources, but cannot create, manage, or subscribe to them\. 

```
   {
        "Sid": "CodeStarNotificationsPowerUserAccess",
        "Effect": "Allow",
        "Action": [
            "codestar-notifications:DescribeNotificationRule"
        ],
        "Resource": "*",
        "Condition" : {
            "StringLike" : {"codestar-notifications:NotificationsForResource" : "arn:aws:codebuild:*"} 
        }
    },    
    {
        "Sid": "CodeStarNotificationsListAccess",
        "Effect": "Allow",
        "Action": [
            "codestar-notifications:ListNotificationRules",
            "codestar-notifications:ListEventTypes",
            "codestar-notifications:ListTargets"
        ],
        "Resource": "*"
    }
```

### Permissions related to notifications in other managed policies<a name="notifications-otheraccess"></a>

The `AWSCodeBuildDeveloperAccess` managed policy includes the following statements to allow users to create, edit, and subscribe to notifications\. Users cannot delete notification rules or manage tags for resources\.

```
    {
        "Sid": "CodeStarNotificationsReadWriteAccess",
        "Effect": "Allow",
        "Action": [
            "codestar-notifications:CreateNotificationRule",
            "codestar-notifications:DescribeNotificationRule",
            "codestar-notifications:UpdateNotificationRule",
            "codestar-notifications:Subscribe",
            "codestar-notifications:Unsubscribe"
        ],
        "Resource": "*",
        "Condition" : {
            "StringLike" : {"codestar-notifications:NotificationsForResource" : "arn:aws:codebuild*"} 
        }
    },    
    {
        "Sid": "CodeStarNotificationsListAccess",
        "Effect": "Allow",
        "Action": [
            "codestar-notifications:ListNotificationRules",
            "codestar-notifications:ListTargets",
            "codestar-notifications:ListTagsforResource",
            "codestar-notifications:ListEventTypes"
        ],
        "Resource": "*"
    },
    {
        "Sid": "SNSTopicListAccess",
        "Effect": "Allow",
        "Action": [
            "sns:ListTopics"
        ],
        "Resource": "*"
    },
    {
        "Sid": "CodeStarNotificationsChatbotAccess",
        "Effect": "Allow",
        "Action": [
            "chatbot:DescribeSlackChannelConfigurations"
          ],
       "Resource": "*"
    }
```

For more information about IAM and notifications, see [Identity and Access Management for AWS CodeStar Notifications](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/security-iam.html)\.

## Customer\-managed policy examples<a name="customer-managed-policies"></a>

In this section, you can find example user policies that grant permissions for AWS CodeBuild actions\. These policies work when you are using the CodeBuild API, AWS SDKs, or AWS CLI\. When you are using the console, you must grant additional, console\-specific permissions\. For information, see [Permissions required to use the AWS CodeBuild console](#console-permissions)\.

You can use the following sample IAM policies to limit CodeBuild access for your IAM users and roles\.

**Topics**
+ [Allow a user to get information about build projects](#customer-managed-policies-example-batch-get-projects)
+ [Allow a user to get information about report groups](#customer-managed-policies-get-information-about-report-group)
+ [Allow a user to get information about reports](#customer-managed-policies-get-information-about-reports)
+ [Allow a user to create build projects](#customer-managed-policies-example-create-project)
+ [Allow a user to create a report group](#customer-managed-policies-example-create-report-group)
+ [Allow a user to delete a report group](#customer-managed-policies-example-delete-report-group)
+ [Allow a user to delete a report](#customer-managed-policies-example-delete-report)
+ [Allow a user to delete build projects](#customer-managed-policies-example-delete-project)
+ [Allow a user to get a list of build project names](#customer-managed-policies-example-list-projects)
+ [Allow a user to change information about build projects](#customer-managed-policies-example-update-project)
+ [Allow a user to change a report group](#customer-managed-policies-example-change-report-group)
+ [Allow a user to get information about builds](#customer-managed-policies-example-batch-get-builds)
+ [Allow a user to get a list of build IDs for a build project](#customer-managed-policies-example-list-builds-for-project)
+ [Allow a user to get a list of build IDs](#customer-managed-policies-example-list-builds)
+ [Allow a user to get a list of report groups](#customer-managed-policies-example-get-list-of-report-groups)
+ [Allow a user to get a list of reports](#customer-managed-policies-example-get-list-of-reports)
+ [Allow a user to get a list of reports for a report group](#customer-managed-policies-example-get-list-of-reports-for-report-group)
+ [Allow a user to get a list of test cases for a report](#customer-managed-policies-example-get-list-of-test-cases-for-report)
+ [Allow a user to start running builds](#customer-managed-policies-example-start-build)
+ [Allow a user to attempt to stop builds](#customer-managed-policies-example-stop-build)
+ [Allow a user to attempt to delete builds](#customer-managed-policies-example-delete-builds)
+ [Allow a user to get information about Docker images that are managed by CodeBuild](#customer-managed-policies-example-list-curated-environment-images)
+ [Allow CodeBuild access to AWS services required to create a VPC network interface](#customer-managed-policies-example-create-vpc-network-interface)
+ [Use a deny statement to prevent AWS CodeBuild from disconnecting from source providers](#customer-managed-policies-example-deny-disconnect)

### Allow a user to get information about build projects<a name="customer-managed-policies-example-batch-get-projects"></a>

The following example policy statement allows a user to get information about build projects in the `us-east-2` Region for account `123456789012` for any build project that starts with the name `my`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:BatchGetProjects",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:project/my*"      
    }
  ]
}
```

### Allow a user to get information about report groups<a name="customer-managed-policies-get-information-about-report-group"></a>

The following example policy statement allows a user to get information about report groups in the `us-east-2` Region for account `123456789012`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:BatchGetReportGroups",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:report-group/*"
    }
  ]
}
```

### Allow a user to get information about reports<a name="customer-managed-policies-get-information-about-reports"></a>

The following example policy statement allows a user to get information about reports in the `us-east-2` Region for account `123456789012`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:BatchGetReports",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:report-group/*"
    }
  ]
}
```

### Allow a user to create build projects<a name="customer-managed-policies-example-create-project"></a>

The following example policy statement allows a user to create build projects with any name but only in the `us-east-2` Region for account `123456789012` and only using the specified CodeBuild service role:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:CreateProject",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:project/*"      
    },
    {
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "arn:aws:iam:123456789012:role/CodeBuildServiceRole"
    }
  ]
}
```

### Allow a user to create a report group<a name="customer-managed-policies-example-create-report-group"></a>

The following example policy statement allows a user to create a report group in the `us-east-2` Region for account `123456789012`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:CreateReportGroup",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:report-group/*"
    }
  ]
}
```

### Allow a user to delete a report group<a name="customer-managed-policies-example-delete-report-group"></a>

The following example policy statement allows a user to delete a report group in the `us-east-2` Region for account `123456789012`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:DeleteReportGroup",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:report-group/*"
    }
  ]
}
```

### Allow a user to delete a report<a name="customer-managed-policies-example-delete-report"></a>

The following example policy statement allows a user to delete a report in the `us-east-2` Region for account `123456789012`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:DeleteReport",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:report-group/*"
    }
  ]
}
```

### Allow a user to delete build projects<a name="customer-managed-policies-example-delete-project"></a>

The following example policy statement allows a user to delete build projects in the `us-east-2` Region for account `123456789012` for any build project that starts with the name `my`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:DeleteProject",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:project/my*"
    }
  ]
}
```

### Allow a user to get a list of build project names<a name="customer-managed-policies-example-list-projects"></a>

The following example policy statement allows a user to get a list of build project names for the same account:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:ListProjects",
      "Resource": "*"
    }
  ]
}
```

### Allow a user to change information about build projects<a name="customer-managed-policies-example-update-project"></a>

The following example policy statement allows a user to change information about build projects with any name but only in the `us-east-2` Region for account `123456789012` and only using the specified AWS CodeBuild service role:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:UpdateProject",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:project/*"
    },
    {
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "arn:aws:iam:123456789012:role/CodeBuildServiceRole"
    }
  ]
}
```

### Allow a user to change a report group<a name="customer-managed-policies-example-change-report-group"></a>

The following example policy statement allows a user to change a report group in the `us-east-2` Region for account `123456789012`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:UpdateReportGroup",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:report-group/*"
    }
  ]
}
```

### Allow a user to get information about builds<a name="customer-managed-policies-example-batch-get-builds"></a>

The following example policy statement allows a user to get information about builds in the `us-east-2` Region for account `123456789012` for the build projects named `my-build-project` and `my-other-build-project`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:BatchGetBuilds",
      "Resource": [
        "arn:aws:codebuild:us-east-2:123456789012:project/my-build-project",
        "arn:aws:codebuild:us-east-2:123456789012:project/my-other-build-project"
      ]
    }
  ]
}
```

### Allow a user to get a list of build IDs for a build project<a name="customer-managed-policies-example-list-builds-for-project"></a>

The following example policy statement allows a user to get a list of build IDs in the `us-east-2` Region for account `123456789012` for the build projects named `my-build-project` and `my-other-build-project`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:ListBuildsForProject",
      "Resource": [
        "arn:aws:codebuild:us-east-2:123456789012:project/my-build-project",
        "arn:aws:codebuild:us-east-2:123456789012:project/my-other-build-project"
      ]
    }
  ]
}
```

### Allow a user to get a list of build IDs<a name="customer-managed-policies-example-list-builds"></a>

The following example policy statement allows a user to get a list of all build IDs for the same account:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:ListBuilds",
      "Resource": "*"
    }
  ]
}
```

### Allow a user to get a list of report groups<a name="customer-managed-policies-example-get-list-of-report-groups"></a>

The following example policy statement allows a user to get a list of report groups in the `us-east-2` Region for account `123456789012`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:ListReportGroups",
      "Resource": "*"
    }
  ]
}
```

### Allow a user to get a list of reports<a name="customer-managed-policies-example-get-list-of-reports"></a>

The following example policy statement allows a user to get a list of reports in the `us-east-2` Region for account `123456789012`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:ListReports",
      "Resource": "*"
    }
  ]
}
```

### Allow a user to get a list of reports for a report group<a name="customer-managed-policies-example-get-list-of-reports-for-report-group"></a>

The following example policy statement allows a user to get a list of reports for a report group in the `us-east-2` Region for account `123456789012`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:ListReportsForReportGroup",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:report-group/*"
    }
  ]
}
```

### Allow a user to get a list of test cases for a report<a name="customer-managed-policies-example-get-list-of-test-cases-for-report"></a>

The following example policy statement allows a user to get a list of test cases for a report in the `us-east-2` Region for account `123456789012`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:DescribeTestCases",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:report-group/*"
    }
  ]
}
```

### Allow a user to start running builds<a name="customer-managed-policies-example-start-build"></a>

The following example policy statement allows a user to run builds in the `us-east-2` Region for account `123456789012` for a build project that starts with the name `my`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:StartBuild",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:project/my*"
    }
  ]
}
```

### Allow a user to attempt to stop builds<a name="customer-managed-policies-example-stop-build"></a>

The following example policy statement allows a user to attempt to stop running builds only in the `us-east-2` region for account `123456789012` for any build project that starts with the name `my`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:StopBuild",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:project/my*"
    }
  ]
}
```

### Allow a user to attempt to delete builds<a name="customer-managed-policies-example-delete-builds"></a>

The following example policy statement allows a user to attempt to delete builds only in the `us-east-2` Region for account `123456789012` for any build project that starts with the name `my`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:BatchDeleteBuilds",
      "Resource": "arn:aws:codebuild:us-east-2:123456789012:project/my*"
    }
  ]
}
```

### Allow a user to get information about Docker images that are managed by CodeBuild<a name="customer-managed-policies-example-list-curated-environment-images"></a>

The following example policy statement allows a user to get information about all Docker images that are managed by CodeBuild:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "codebuild:ListCuratedEnvironmentImages",
      "Resource": "*"
    }
  ]
}
```

### Allow CodeBuild access to AWS services required to create a VPC network interface<a name="customer-managed-policies-example-create-vpc-network-interface"></a>

The following example policy statement grants AWS CodeBuild permission to create a network interface in a VPC with two subnets:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow", 
            "Action": [
                "ec2:CreateNetworkInterface",
                "ec2:DescribeDhcpOptions",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DeleteNetworkInterface",
                "ec2:DescribeSubnets",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeVpcs"
            ], 
            "Resource": "*" 
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateNetworkInterfacePermission"
            ],
            "Resource": "arn:aws:ec2:region:account-id:network-interface/*",
            "Condition": {
                "StringEquals": {
                    "ec2:Subnet": [
                        "arn:aws:ec2:region:account-id:subnet/subnet-id-1",
                        "arn:aws:ec2:region:account-id:subnet/subnet-id-2"
                    ],
                    "ec2:AuthorizedService": "codebuild.amazonaws.com"
                }
            }
        }
    ]
}
```

### Use a deny statement to prevent AWS CodeBuild from disconnecting from source providers<a name="customer-managed-policies-example-deny-disconnect"></a>

 The following example policy statement uses a deny statement to prevent AWS CodeBuild from disconnecting from source providers\. It uses `codebuild:DeleteOAuthToken`, which is the inverse of `codebuild:PersistOAuthToken` and `codebuild:ImportSourceCredentials`, to connect with source providers\. For more information, see [Permissions required for the AWS CodeBuild console to connect to source providers](#console-policies)\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "codebuild:DeleteOAuthToken",
      "Resource": "*"
    }
  ]
}
```