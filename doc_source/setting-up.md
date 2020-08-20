# Advanced setup<a name="setting-up"></a>

If you follow the steps in [Getting started using the console](getting-started.md) to access AWS CodeBuild for the first time, you most likely do not need the information in this topic\. However, as you continue using CodeBuild, you might want to do things such as give IAM groups and users in your organization access to CodeBuild, modify existing service roles in IAM or customer master keys in AWS KMS to access CodeBuild, or set up the AWS CLI across your organization's workstations to access CodeBuild\. This topic describes how to complete the related setup steps\.

We assume you already have an AWS account\. However, if you do not already have one, go to [http://aws\.amazon\.com](http://aws.amazon.com), choose **Sign In to the Console**, and follow the online instructions\.

**Topics**
+ [Add CodeBuild access permissions to an IAM group or IAM user](#setting-up-service-permissions-group)
+ [Create a CodeBuild service role](#setting-up-service-role)
+ [Create and configure an AWS KMS CMK for CodeBuild](#setting-up-kms)
+ [Install and configure the AWS CLI](#setting-up-cli)

## Add CodeBuild access permissions to an IAM group or IAM user<a name="setting-up-service-permissions-group"></a>

To access AWS CodeBuild with an IAM group or IAM user, you must add access permissions\. This section describes how to do this with the IAM console or the AWS CLI\.

If you will access CodeBuild with your AWS root account \(not recommended\) or an administrator IAM user in your AWS account, then you do not need to follow these instructions\.

For information about AWS root accounts and administrator IAM users, see [The Account Root User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) and [Creating Your First IAM Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.<a name="setting-up-service-permissions-group-console"></a>

**To add CodeBuild access permissions to an IAM group or IAM user \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   You should have already signed in to the AWS Management Console by using one of the following:
   + Your AWS root account\. This is not recommended\. For more information, see [The Account Root User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in the *IAM User Guide*\.
   + An administrator IAM user in your AWS account\. For more information, see [Creating Your First IAM Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
   + An IAM user in your AWS account with permission to perform the following minimum set of actions:

     ```
     iam:AttachGroupPolicy
     iam:AttachUserPolicy
     iam:CreatePolicy
     iam:ListAttachedGroupPolicies
     iam:ListAttachedUserPolicies
     iam:ListGroups
     iam:ListPolicies
     iam:ListUsers
     ```

     For more information, see [Overview of IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.

1. In the navigation pane, choose **Policies**\.

1. To add a custom set of AWS CodeBuild access permissions to an IAM group or IAM user, skip ahead to step 4 in this procedure\.

   To add a default set of CodeBuild access permissions to an IAM group or IAM user, choose **Policy Type**, **AWS Managed**, and then do the following:
   + To add full access permissions to CodeBuild, select the box named **AWSCodeBuildAdminAccess**, choose **Policy Actions**, and then choose **Attach**\. Select the box next to the target IAM group or IAM user, and then choose **Attach Policy**\. Repeat this for the policies named **AmazonS3ReadOnlyAccess** and **IAMFullAccess**\.
   + To add access permissions to CodeBuild for everything except build project administration, select the box named **AWSCodeBuildDeveloperAccess**, choose **Policy Actions**, and then choose **Attach**\. Select the box next to the target IAM group or IAM user, and then choose **Attach Policy**\. Repeat this for the policy named **AmazonS3ReadOnlyAccess**\.
   + To add read\-only access permissions to CodeBuild, select the boxes named **AWSCodeBuildReadOnlyAccess**\. Select the box next to the target IAM group or IAM user, and then choose **Attach Policy**\. Repeat this for the policy named **AmazonS3ReadOnlyAccess**\.

   You have now added a default set of CodeBuild access permissions to an IAM group or IAM user\. Skip the rest of the steps in this procedure\.

1. Choose **Create Policy**\.

1. On the **Create Policy** page, next to **Create Your Own Policy**, choose **Select**\.

1. On the **Review Policy** page, for **Policy Name**, enter a name for the policy \(for example, **CodeBuildAccessPolicy**\)\. If you use a different name, be sure to use it throughout this procedure\.

1. For **Policy Document**, enter the following, and then choose **Create Policy**\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "CodeBuildDefaultPolicy",
         "Effect": "Allow",
         "Action": [
           "codebuild:*",
           "iam:PassRole"
         ],
         "Resource": "*"      
       },
       {
         "Sid": "CloudWatchLogsAccessPolicy",
         "Effect": "Allow",
         "Action": [
           "logs:FilterLogEvents",
           "logs:GetLogEvents"
         ],
         "Resource": "*"
       },
       {
         "Sid": "S3AccessPolicy",
         "Effect": "Allow",
         "Action": [
           "s3:CreateBucket",
           "s3:GetObject",
           "s3:List*",
           "s3:PutObject"
         ],
         "Resource": "*"
       },
       {
         "Sid": "S3BucketIdentity",
         "Effect": "Allow",
         "Action": [
           "s3:GetBucketAcl",
           "s3:GetBucketLocation"
         ],
         "Resource": "*"
       }
     ]
   }
   ```
**Note**  
This policy allows access to all CodeBuild actions and to a potentially large number of AWS resources\. To restrict permissions to specific CodeBuild actions, change the value of `codebuild:*` in the CodeBuild policy statement\. For more information, see [Identity and access management](auth-and-access-control.md)\. To restrict access to specific AWS resources, change the value of the `Resource` object\. For more information, see [Identity and access management](auth-and-access-control.md)\.

1. In the navigation pane, choose **Groups** or **Users**\.

1. In the list of groups or users, choose the name of the IAM group or IAM user to which you want to add CodeBuild access permissions\.

1. For a group, on the group settings page, on the **Permissions** tab, expand **Managed Policies**, and then choose **Attach Policy**\.

   For a user, on the user settings page, on the **Permissions** tab, choose **Add permissions**\.

1. For a group, on the **Attach Policy** page, select **CodeBuildAccessPolicy**, and then choose **Attach Policy**\.

   For a user, on the **Add permisions** page, choose **Attach existing policies directly**\. Select **CodeBuildAccessPolicy**, choose **Next: Reivew**, and then choose **Add permissions**\.<a name="setting-up-service-permissions-group-cli"></a>

**To add CodeBuild access permissions to an IAM group or IAM user \(AWS CLI\)**

1. Make sure you have configured the AWS CLI with the AWS access key and AWS secret access key that correspond to one of the IAM entities, as described in the previous procedure\. For more information, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

1. To add a custom set of AWS CodeBuild access permissions to an IAM group or IAM user, skip to step 3 in this procedure\.

   To add a default set of CodeBuild access permissions to an IAM group or IAM user, do the following:

   Run one of the following commands, depending on whether you want to add permissions to an IAM group or IAM user:

   ```
   aws iam attach-group-policy --group-name group-name --policy-arn policy-arn
   
   aws iam attach-user-policy --user-name user-name --policy-arn policy-arn
   ```

   You must run the command three times, replacing *group\-name* or *user\-name* with the IAM group name or IAM user name, and replacing *policy\-arn* once for each of the following policy Amazon Resource Names \(ARNs\): 
   + To add full access permissions to CodeBuild, use the following policy ARNs:
     + `arn:aws:iam::aws:policy/AWSCodeBuildAdminAccess`
     + `arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess`
     + `arn:aws:iam::aws:policy/IAMFullAccess`
   + To add access permissions to CodeBuild for everything except build project administration, use the following policy ARNs:
     + `arn:aws:iam::aws:policy/AWSCodeBuildDeveloperAccess`
     + `arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess`
   + To add read\-only access permissions to CodeBuild, use the following policy ARNs:
     + `arn:aws:iam::aws:policy/AWSCodeBuildReadOnlyAccess`
     + `arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess`

   You have now added a default set of CodeBuild access permissions to an IAM group or IAM user\. Skip the rest of the steps in this procedure\.

1. In an empty directory on the local workstation or instance where the AWS CLI is installed, create a file named `put-group-policy.json` or `put-user-policy.json`\. If you use a different file name, be sure to use it throughout this procedure\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "CodeBuildAccessPolicy",
         "Effect": "Allow",
         "Action": [
           "codebuild:*",
           "iam:PassRole"
         ],
         "Resource": "*"
       },
       {
         "Sid": "CloudWatchLogsAccessPolicy",
         "Effect": "Allow",
         "Action": [
           "logs:FilterLogEvents",
           "logs:GetLogEvents"
         ],
         "Resource": "*"
       },
       {
         "Sid": "S3AccessPolicy",
         "Effect": "Allow",
         "Action": [
           "s3:CreateBucket",
           "s3:GetObject",
           "s3:List*",
           "s3:PutObject"
         ],
         "Resource": "*"
       },
       {
         "Sid": "S3BucketIdentity",
         "Effect": "Allow",
         "Action": [
           "s3:GetBucketAcl",
           "s3:GetBucketLocation"
         ],
         "Resource": "*"
       }
     ]
   }
   ```
**Note**  
This policy allows access to all CodeBuild actions and to a potentially large number of AWS resources\. To restrict permissions to specific CodeBuild actions, change the value of `codebuild:*` in the CodeBuild policy statement\. For more information, see [Identity and access management](auth-and-access-control.md)\. To restrict access to specific AWS resources, change the value of the related `Resource` object\. For more information, see [Identity and access management](auth-and-access-control.md) or the specific AWS service's security documentation\.

1. Switch to the directory where you saved the file, and then run one of the following commands\. You can use different values for `CodeBuildGroupAccessPolicy` and `CodeBuildUserAccessPolicy`\. If you use different values, be sure to use them here\.

   For an IAM group:

   ```
   aws iam put-group-policy --group-name group-name --policy-name CodeBuildGroupAccessPolicy --policy-document file://put-group-policy.json
   ```

   For an IAM user:

   ```
   aws iam put-user-policy --user-name user-name --policy-name CodeBuildUserAccessPolicy --policy-document file://put-user-policy.json
   ```

   In the preceding commands, replace *group\-name* or *user\-name* with the name of the target IAM group or IAM user\.

## Create a CodeBuild service role<a name="setting-up-service-role"></a>

You need an AWS CodeBuild service role so that CodeBuild can interact with dependent AWS services on your behalf\. You can create a CodeBuild service role by using the CodeBuild or AWS CodePipeline consoles\. For information, see:
+ [Create a build project \(console\)](create-project-console.md)
+ [Create a pipeline that uses CodeBuild \(CodePipeline console\)](how-to-create-pipeline.md#how-to-create-pipeline-console)
+ [Add a CodeBuild build action to a pipeline \(CodePipeline console\)](how-to-create-pipeline.md#how-to-create-pipeline-add)
+ [Change a build project's settings \(console\)](change-project-console.md)

If you do not plan to use these consoles, this section describes how to create a CodeBuild service role with the IAM console or the AWS CLI\. 

**Note**  
The service role described on this page contains a policy that grants the minimum permissions required to use CodeBuild\. You might need to add additional permissions depending on your use case\. For example, if you want to use CodeBuild with Amazon Virtual Private Cloud, then the service role you create requires the permissions in the following policy: [Create a CodeBuild service role](#setting-up-service-role)\.<a name="setting-up-service-role-console"></a>

**To create a CodeBuild service role \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   You should have already signed in to the console by using one of the following:
   + Your AWS root account\. This is not recommended\. For more information, see [The Account Root User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in the *IAM User Guide*\.
   + An administrator IAM user in your AWS account\. For more information, see [Creating Your First IAM Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
   + An IAM user in your AWS account with permission to perform the following minimum set of actions:

     ```
     iam:AddRoleToInstanceProfile
     iam:AttachRolePolicy
     iam:CreateInstanceProfile
     iam:CreatePolicy
     iam:CreateRole
     iam:GetRole
     iam:ListAttachedRolePolicies
     iam:ListPolicies
     iam:ListRoles
     iam:PassRole
     iam:PutRolePolicy
     iam:UpdateAssumeRolePolicy
     ```

     For more information, see [Overview of IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.

1. In the navigation pane, choose **Policies**\.

1. Choose **Create Policy**\.

1. On the **Create Policy** page, choose **JSON**\.

1. For the JSON policy, enter the following, and then choose **Review Policy**:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "CloudWatchLogsPolicy",
         "Effect": "Allow",
         "Action": [
           "logs:CreateLogGroup",
           "logs:CreateLogStream",
           "logs:PutLogEvents"
         ],
         "Resource": [
           "*"
         ]
       },
       {
         "Sid": "CodeCommitPolicy",
         "Effect": "Allow",
         "Action": [
           "codecommit:GitPull"
         ],
         "Resource": [
           "*"
         ]
       },
       {
         "Sid": "S3GetObjectPolicy",
         "Effect": "Allow",
         "Action": [
           "s3:GetObject",
           "s3:GetObjectVersion"
         ],
         "Resource": [
           "*"
         ]
       },
       {
         "Sid": "S3PutObjectPolicy",
         "Effect": "Allow",
         "Action": [
           "s3:PutObject"
         ],
         "Resource": [
           "*"
         ]
       },
       {
         "Sid": "ECRPullPolicy",
         "Effect": "Allow",
         "Action": [
           "ecr:BatchCheckLayerAvailability",
           "ecr:GetDownloadUrlForLayer",
           "ecr:BatchGetImage"
         ],
         "Resource": [
           "*"
         ]
       },
       {
         "Sid": "ECRAuthPolicy",
         "Effect": "Allow",
         "Action": [
           "ecr:GetAuthorizationToken"
         ],
         "Resource": [
           "*"
         ]
       },
       {
         "Sid": "S3BucketIdentity",
         "Effect": "Allow",
         "Action": [
           "s3:GetBucketAcl",
           "s3:GetBucketLocation"
         ],
         "Resource": 
           "*"
       }
     ]
   }
   ```
**Note**  
This policy contains statements that allow access to a potentially large number of AWS resources\. To restrict AWS CodeBuild to access specific AWS resources, change the value of the `Resource` array\. For more information, see the security documentation for the AWS service\.

1. On the **Review Policy** page, for **Policy Name**, enter a name for the policy \(for example, **CodeBuildServiceRolePolicy**\), and then choose **Create policy**\.
**Note**  
If you use a different name, be sure to use it throughout this procedure\.

1. In the navigation pane, choose **Roles**\.

1. Choose **Create role**\.

1. On the **Create role** page, with **AWS Service** already selected, choose **CodeBuild**, and then choose **Next:Permissions**\.

1. On the **Attach permissions policies** page, select **CodeBuildServiceRolePolicy**, and then choose **Next: Review**\.

1. On the **Create role and review** page, for **Role name**, enter a name for the role \(for example, **CodeBuildServiceRole**\), and then choose **Create role**\.<a name="setting-up-service-role-cli"></a>

**To create a CodeBuild service role \(AWS CLI\)**

1. Make sure you have configured the AWS CLI with the AWS access key and AWS secret access key that correspond to one of the IAM entities, as described in the previous procedure\. For more information, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

1. In an empty directory on the local workstation or instance where the AWS CLI is installed, create two files named `create-role.json` and `put-role-policy.json`\. If you choose different file names, be sure to use them throughout this procedure\.

   `create-role.json`:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "codebuild.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

   `put-role-policy.json`:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "CloudWatchLogsPolicy",
         "Effect": "Allow",
         "Action": [
           "logs:CreateLogGroup",
           "logs:CreateLogStream",
           "logs:PutLogEvents"
         ],
         "Resource": [
           "*"
         ]
       },
       {
         "Sid": "CodeCommitPolicy",
         "Effect": "Allow",
         "Action": [
           "codecommit:GitPull"
         ],
         "Resource": [
           "*"
         ]
       },
       {
         "Sid": "S3GetObjectPolicy",
         "Effect": "Allow",
         "Action": [
           "s3:GetObject",
           "s3:GetObjectVersion"
         ],
         "Resource": [
           "*"
         ]
       },
       {
         "Sid": "S3PutObjectPolicy",
         "Effect": "Allow",
         "Action": [
           "s3:PutObject"
         ],
         "Resource": [
           "*"
         ]
       },
       {
         "Sid": "S3BucketIdentity",
         "Effect": "Allow",
         "Action": [
           "s3:GetBucketAcl",
           "s3:GetBucketLocation"
         ],
         "Resource": [
           "*"
         ]
       }
     ]
   }
   ```
**Note**  
This policy contains statements that allow access to a potentially large number of AWS resources\. To restrict AWS CodeBuild to access specific AWS resources, change the value of the `Resource` array\. For more information, see the security documentation for the AWS service\.

1. Switch to the directory where you saved the preceding files, and then run the following two commands, one at a time, in this order\. You can use different values for `CodeBuildServiceRole` and `CodeBuildServiceRolePolicy`, but be sure to use them here\.

   ```
   aws iam create-role --role-name CodeBuildServiceRole --assume-role-policy-document file://create-role.json
   ```

   ```
   aws iam put-role-policy --role-name CodeBuildServiceRole --policy-name CodeBuildServiceRolePolicy --policy-document file://put-role-policy.json
   ```

## Create and configure an AWS KMS CMK for CodeBuild<a name="setting-up-kms"></a>

For AWS CodeBuild to encrypt its build output artifacts, it needs access to an AWS KMS customer master key \(CMK\)\. By default, CodeBuild uses the AWS\-managed CMK for Amazon S3 in your AWS account\.

If you do not want to use this CMK, you must create and configure a customer\-managed CMK yourself\. This section describes how to do this with the IAM console\.

For information about CMKs, see [AWS Key Management Service Concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html) and [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS KMS Developer Guide*\.

To configure a CMK for use by CodeBuild, follow the instructions in the "How to Modify a Key Policy" section of [Modifying a Key Policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policy-modifying.html) in the *AWS KMS Developer Guide*\. Then add the following statements \(between *\#\#\# BEGIN ADDING STATEMENTS HERE \#\#\#* and *\#\#\# END ADDING STATEMENTS HERE \#\#\#*\) to the key policy\. Ellipses \(`...`\) are used for brevity and to help you locate where to add the statements\. Do not remove any statements, and do not type these ellipses into the key policy\.

```
{
  "Version": "2012-10-17",
  "Id": "...",
  "Statement": [
    ### BEGIN ADDING STATEMENTS HERE ###
    {
      "Sid": "Allow access through Amazon S3 for all principals in the account that are authorized to use Amazon S3",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "kms:ViaService": "s3.region-ID.amazonaws.com",
          "kms:CallerAccount": "account-ID"
        }
      }
    },
    {
      "Effect": "Allow", 
      "Principal": {
        "AWS": "arn:aws:iam::account-ID:role/CodeBuild-service-role"
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
      ],
      "Resource": "*"
    },
    ### END ADDING STATEMENTS HERE ###
    {
      "Sid": "Enable IAM User Permissions",
      ...
    },
    {
      "Sid": "Allow access for Key Administrators",
      ...
    },
    {
      "Sid": "Allow use of the key",
      ...
    },
    {
      "Sid": "Allow attachment of persistent resources",
      ...
    }
  ]
}
```
+ *region\-ID* represents the ID of the AWS region where the Amazon S3 buckets associated with CodeBuild are located \(for example, `us-east-1`\)\.
+ *account\-ID* represents the ID of the of the AWS account that owns the CMK\.
+ *CodeBuild\-service\-role* represents the name of the CodeBuild service role you created or identified earlier in this topic\.

**Note**  
To create or configure a CMK through the IAM console, you must first sign in to the AWS Management Console by using one of the following:  
Your AWS root account\. This is not recommended\. For more information, see [The Account Root User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in the *IAM User Guide*\.
An administrator IAM user in your AWS account\. For more information, see [Creating Your First IAM Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
An IAM user in your AWS account with permission to create or modify the CMK\. For more information, see [Permissions Required to Use the AWS KMS Console](https://docs.aws.amazon.com/kms/latest/developerguide/iam-policies.html#console-permissions) in the *AWS KMS Developer Guide*\.

## Install and configure the AWS CLI<a name="setting-up-cli"></a>

To access AWS CodeBuild, you can use the AWS CLI with—or instead of—the CodeBuild console, the CodePipeline console, or the AWS SDKs\. To install and configure the AWS CLI, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

1. Run the following command to confirm whether your installation of the AWS CLI supports CodeBuild:

   ```
   aws codebuild list-builds
   ```

   If successful, information similar to the following will appear in the output:

   ```
   {
     "ids": []
   }
   ```

   The empty square brackets indicate that you have not yet run any builds\.

1. If an error is output, you must uninstall your current version of the AWS CLI and then install the latest version\. For more information, see [Uninstalling the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-uninstall.html) and [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.