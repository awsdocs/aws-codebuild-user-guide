# Using Identity\-Based Policies \(IAM Policies\) for AWS CodeBuild<a name="auth-and-access-control-iam-identity-based-access-control"></a>

This topic provides examples of identity\-based policies that demonstrate how an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\) and thereby grant permissions to perform operations on AWS CodeBuild resources\.

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your AWS CodeBuild resources\. For more information, see [Overview of Managing Access Permissions to Your AWS CodeBuild Resources](auth-and-access-control-iam-access-control-identity-based.md)\.

**Topics**
+ [Permissions Required to Use the AWS CodeBuild Console](#console-permissions)
+ [Permissions Required for the AWS CodeBuild Console to Connect to Source Providers Using OAuth](#console-policies)
+ [AWS Managed \(Predefined\) Policies for AWS CodeBuild](#managed-policies)
+ [Customer\-Managed Policy Examples](#customer-managed-policies)

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

## Permissions Required to Use the AWS CodeBuild Console<a name="console-permissions"></a>

A user who uses the AWS CodeBuild console must have a minimum set of permissions that allows the user to describe other AWS resources for the AWS account\. You must have permissions from the following services:
+ AWS CodeBuild
+ Amazon CloudWatch
+ AWS CodeCommit \(if you are storing your source code in an AWS CodeCommit repository\)
+ Amazon Elastic Container Registry \(Amazon ECR\) \(if you are using a build environment that relies on a Docker image in an Amazon ECR repository\)
+ Amazon Elastic Container Service \(Amazon ECS\) \(if you are using a build environment that relies on a Docker image in an Amazon ECR repository\)
+ AWS Identity and Access Management \(IAM\)
+ AWS Key Management Service \(AWS KMS\)
+ Amazon Simple Storage Service \(Amazon S3\)

If you create an IAM policy that is more restrictive than the minimum required permissions, the console won't function as intended\.

## Permissions Required for the AWS CodeBuild Console to Connect to Source Providers Using OAuth<a name="console-policies"></a>

The AWS CodeBuild console uses the following API actions to connect to source providers \(for example, GitHub repositories\) using OAuth\.
+ `codebuild:ListConnectedOAuthAccounts`
+ `codebuild:ListRepositories`
+ `codebuild:PersistOAuthToken`

To associate source providers using OAuth \(such as GitHub repositories\) with your build projects, you must use the AWS CodeBuild console\. To do this, you must first add the preceding API actions to IAM access policies associated with the IAM user you use to access the AWS CodeBuild console\.

These API actions are not intended to be called by your code\. Therefore, these API actions are not included in the AWS CLI and AWS SDKs\.

## AWS Managed \(Predefined\) Policies for AWS CodeBuild<a name="managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so you can avoid having to investigate what permissions are needed\. For more information, see [AWS Managed Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

The following AWS managed policies, which you can attach to users in your account, are specific to AWS CodeBuild\.
+ **AWSCodeBuildAdminAccess** – Provides full access to AWS CodeBuild including permissions to administrate AWS CodeBuild build projects\. 
+ **AWSCodeBuildDeveloperAccess** – Provides access to AWS CodeBuild but does not allow build project administration\.
+ **AWSCodeBuildReadOnlyAccess** – Provides read\-only access to AWS CodeBuild\.

To access build output artifacts that AWS CodeBuild creates, you must also attach the AWS managed policy named **AmazonS3ReadOnlyAccess**\.

To create and manage AWS CodeBuild service roles, you must also attach the AWS managed policy named **IAMFullAccess**\.

You can also create your own custom IAM policies to allow permissions for AWS CodeBuild actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\.

## Customer\-Managed Policy Examples<a name="customer-managed-policies"></a>

In this section, you can find example user policies that grant permissions for AWS CodeBuild actions\. These policies work when you are using the AWS CodeBuild API, AWS SDKs, or AWS CLI\. When you are using the console, you must grant additional permissions specific to the console\. For information, see [Permissions Required to Use the AWS CodeBuild Console](#console-permissions)\.

You can use the following sample IAM policies to limit AWS CodeBuild access for your IAM users and roles\.

**Topics**
+ [Allow a User to Get Information About Build Projects](#customer-managed-policies-example-batch-get-projects)
+ [Allow a User to Create Build Projects](#customer-managed-policies-example-create-project)
+ [Allow a User to Delete Build Projects](#customer-managed-policies-example-delete-project)
+ [Allow a User to Get a List of Build Project Names](#customer-managed-policies-example-list-projects)
+ [Allow a User to Change Information About Build Projects](#customer-managed-policies-example-update-project)
+ [Allow a User to Get Information About Builds](#customer-managed-policies-example-batch-get-builds)
+ [Allow a User to Get a List of Build IDs for a Build Project](#customer-managed-policies-example-list-builds-for-project)
+ [Allow a User to Get a List of Build IDs](#customer-managed-policies-example-list-builds)
+ [Allow a User to Begin Running Builds](#customer-managed-policies-example-start-build)
+ [Allow a User to Attempt to Stop Builds](#customer-managed-policies-example-stop-build)
+ [Allow a User to Attempt to Delete Builds](#customer-managed-policies-example-delete-builds)
+ [Allow a User to Get Information About Docker Images that Are Managed by AWS CodeBuild](#customer-managed-policies-example-list-curated-environment-images)
+ [Allow a User to Create a VPC Network Interface](#customer-managed-policies-example-create-vpc-network-interface)

### Allow a User to Get Information About Build Projects<a name="customer-managed-policies-example-batch-get-projects"></a>

The following example policy statement allows a user to get information about build projects only in the `us-east-2` region for account `123456789012` for any build project that starts with the name `my`:

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

### Allow a User to Create Build Projects<a name="customer-managed-policies-example-create-project"></a>

The following example policy statement allows a user to create build projects with any name but only in the `us-east-2` region for account `123456789012` and using only the specified AWS CodeBuild service role:

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

### Allow a User to Delete Build Projects<a name="customer-managed-policies-example-delete-project"></a>

The following example policy statement allows a user to delete build projects only in the `us-east-2` region for account `123456789012` for any build project that starts with the name `my`:

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

### Allow a User to Get a List of Build Project Names<a name="customer-managed-policies-example-list-projects"></a>

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

### Allow a User to Change Information About Build Projects<a name="customer-managed-policies-example-update-project"></a>

The following example policy statement allows a user to change information about build projects with any name but only in the `us-east-2` region for account `123456789012` and using only the specified AWS CodeBuild service role:

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

### Allow a User to Get Information About Builds<a name="customer-managed-policies-example-batch-get-builds"></a>

The following example policy statement allows a user to get information about builds only in the `us-east-2` region for account `123456789012` for the build projects named `my-build-project` and `my-other-build-project`:

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

### Allow a User to Get a List of Build IDs for a Build Project<a name="customer-managed-policies-example-list-builds-for-project"></a>

The following example policy statement allows a user to get a list of build IDs only in the `us-east-2` region for account `123456789012` for the build projects named `my-build-project` and `my-other-build-project`:

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

### Allow a User to Get a List of Build IDs<a name="customer-managed-policies-example-list-builds"></a>

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

### Allow a User to Begin Running Builds<a name="customer-managed-policies-example-start-build"></a>

The following example policy statement allows a user to run builds only in the `us-east-2` region for account `123456789012` for build project that starts with the name `my`:

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

### Allow a User to Attempt to Stop Builds<a name="customer-managed-policies-example-stop-build"></a>

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

### Allow a User to Attempt to Delete Builds<a name="customer-managed-policies-example-delete-builds"></a>

The following example policy statement allows a user to attempt to delete builds only in the `us-east-2` region for account `123456789012` for any build project that starts with the name `my`:

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

### Allow a User to Get Information About Docker Images that Are Managed by AWS CodeBuild<a name="customer-managed-policies-example-list-curated-environment-images"></a>

The following example policy statement allows a user to get information about all Docker images that are managed by AWS CodeBuild:

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

### Allow a User to Create a VPC Network Interface<a name="customer-managed-policies-example-create-vpc-network-interface"></a>

The following example policy statement allows a user to create a network interface in an Amazon VPC:

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
            "Resource": "arn:aws:ec2:{{region}}:{{account-id}}:network-interface/*",
            "Condition": {
                "StringEquals": {
                    "ec2:Subnet": [
                        "arn:aws:ec2:{{region}}:{{account-id}}:subnet/[[subnets]]"
                    ],
                    "ec2:AuthorizedService": "codebuild.amazonaws.com"
                }
            }
        }
    ]
}
```